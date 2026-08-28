# 11 · Flujo de generación y calendario de eventos

Cómo el **motor de formatos** convierte un evento con inscripciones en enfrentamientos
programados. Aplica el patrón Strategy por `formato` (ver [ADR-0001](./adr/0001-motor-de-formatos.md)).

## Principio de diseño: pasos explícitos, no automáticos por temporizador

La generación **se dispara con botones del admin**, no sola al llegar una fecha. Esto da
control (equipos que no pagaron, imprevistos, etc.). Además, **generar enfrentamientos** y
**asignar fechas** son **dos pasos separados**:

```
1. Cerrar inscripciones      (admin)
2. Generar enfrentamientos   (motor de formatos) → quién juega contra quién, SIN fecha
3. Programar calendario       → el sistema PROPONE fechas, el admin AJUSTA
4. Iniciar evento            → queda EN_CURSO, visible para jugadores
```

## Ciclo de vida con acciones

```
BORRADOR
  │  (admin configura: tipo, formato, cupo, cuota)
  ▼
INSCRIPCIONES  ──► equipos se inscriben (aprobación / pago si aplica)
  │  [Botón] Cerrar inscripciones
  ▼
GENERACIÓN (pre-inicio)
  │  [Botón] Generar enfrentamientos  ← motor de formatos
  │  [Botón] Re-sortear (solo eliminación, antes de iniciar)
  │  [Botón] Proponer calendario → admin ajusta fechas/horarios/canal
  │  [Botón] Iniciar evento
  ▼
EN_CURSO  ──► staff captura resultados → avanza bracket / recalcula tabla
  │  [Botón] Generar playoffs (solo Temporada, tras fase regular)
  │  [Botón] Finalizar
  ▼
FINALIZADO
```

## Generación por formato

### 🏆 Temporada — `ROUND_ROBIN` + playoffs

1. **Generar fase regular:** el motor crea **todos contra todos** y los organiza en
   **jornadas**. Los enfrentamientos son **deterministas** (no random): con N equipos hay
   N·(N−1)/2 partidos. Lo único variable es el **orden de las jornadas** (calendario).
   - Con N impar se introduce un *bye* por jornada.
2. **Tabla de posiciones** arranca en ceros; se recalcula con cada resultado
   (victorias, derrotas, diferencia de rondas — criterios de desempate configurables).
3. **Generar playoffs** (acción posterior): toma los **mejores N** de la tabla y arma un
   bracket de eliminación con **seeds por posición** (1º vs último, etc.).

### ⚡ Relámpago — `SINGLE_ELIM`

1. **Generar bracket (sorteo):** el motor hace un **sorteo aleatorio** de los equipos
   aprobados y arma las llaves.
   - Si el número de equipos no es potencia de 2, se asignan **byes** en la primera ronda.
2. **Re-sortear:** el admin puede **volver a sortear** cuantas veces quiera **antes de
   iniciar** el evento. Una vez iniciado, el bracket queda fijo.
3. Al capturar un resultado, el ganador **avanza** a `siguientePartidoId`.

> `DOUBLE_ELIM` y `LEAGUE_PLAYOFFS` se implementan igual (estrategias adicionales); en el
> MVP se priorizan `ROUND_ROBIN` y `SINGLE_ELIM`.

## Calendario: automático + ajuste manual

Tras generar los enfrentamientos (sin fecha), el admin pulsa **"Proponer calendario"**:

1. El sistema genera una **propuesta base** según parámetros del evento:
   - Fecha de inicio, cadencia (ej. *1 jornada por semana* en liga; *todas las rondas el
     mismo día* en relámpago), horario tipo y **canal de transmisión** por default.
2. La propuesta se muestra editable: el admin **ajusta** fecha/hora/canal de cualquier
   partido o jornada.
3. Al confirmar, cada `Partido` queda con `fechaProgramada` y `canalTransmisionId`.

**Parámetros de la propuesta (configurables por evento):**
- Fecha/hora de arranque.
- Cadencia (días entre jornadas / rondas).
- Ventana horaria preferida (ej. 20:00–23:00).
- Canal(es) de transmisión disponibles y su asignación por default.

> Los cambios de calendario disparan notificación push a los equipos afectados y webhook
> a n8n (recordatorio en Discord). Ver [10-notificaciones-y-automatizacion.md](./10-notificaciones-y-automatizacion.md).

## Botones del Admin Web (resumen)

| Botón | Cuándo aparece | Qué hace |
|-------|----------------|----------|
| Cerrar inscripciones | Estado `INSCRIPCIONES` | Bloquea nuevas inscripciones |
| Generar enfrentamientos | Inscripciones cerradas | Crea partidos según formato |
| Re-sortear | Eliminación, antes de iniciar | Rehace el sorteo aleatorio |
| Proponer calendario | Enfrentamientos generados | Crea fechas propuestas (editables) |
| Iniciar evento | Calendario listo | Pasa a `EN_CURSO`, visible en la app |
| Generar playoffs | Temporada, fase regular terminada | Arma bracket final con top N |
| Finalizar | `EN_CURSO` | Cierra el evento |

## Invariantes (para tests)

- [ ] Solo inscripciones `APROBADA` (+`PAGADO` si cuota) entran a la generación.
- [ ] Round-robin genera exactamente N·(N−1)/2 partidos (con bye si N impar).
- [ ] Single-elim maneja byes cuando N no es potencia de 2.
- [ ] Re-sortear solo permitido **antes** de `EN_CURSO`.
- [ ] "Proponer calendario" nunca deja partidos sin `fechaProgramada` tras confirmar.
- [ ] Cambiar de estado a `EN_CURSO` congela el bracket/fixture.
