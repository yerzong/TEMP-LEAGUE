# 08 · Spec — Consola Admin/Staff (React web)

Herramienta de **operación** de la liga. La usan `ADMIN` (control total) y `STAFF/CASTER`
(transmitir y capturar resultados). Aquí vive **toda** la gestión que NO está en la app.

## Módulos

```
[Login admin/staff]
 └─ Dashboard
     ├─ Eventos            (crear, editar, ciclo de vida)
     ├─ Inscripciones      (aprobar/rechazar, seeds, pago)
     ├─ Bracket/Calendario (generar, programar, asignar canal)
     ├─ Resultados         (captura de marcadores)  ← núcleo del STAFF
     ├─ Canales            (transmisiones de la liga)
     ├─ Organizaciones     (marcas y rosters)
     ├─ Jugadores          (usuarios + bitácora de conducta)
     ├─ Moderación         (reportes y sanciones)
     ├─ Staff              (invitar/gestionar admins y casters)
     ├─ Auditoría          (log de acciones sensibles)
     └─ (v2) Contenido/IA  (resúmenes, posts) · Reportes analíticos
```

## Módulos — detalle

### Dashboard
- Resumen: eventos activos, **inscripciones pendientes**, **partidos de hoy**, alertas.
- Accesos rápidos a capturar resultado y aprobar inscripciones.

### Gestión de eventos (ADMIN)
- **Crear/editar evento**: nombre, **tipo** (Temporada/Relámpago), **formato**, fechas,
  `maxEquipos`, **cuotaEntrada** (0 = gratis) y moneda.
- Ciclo de vida: **Abrir inscripciones** → **Iniciar** (genera bracket/tabla vía motor de
  formatos) → **Finalizar**.

### Inscripciones (ADMIN)
- Lista de equipos inscritos por evento con su estado.
- **Aprobar / rechazar**. Asignar **seed**.
- Ver **estado de pago** si el evento tiene cuota.

### Bracket / Calendario (ADMIN/STAFF)
- Visualizar el bracket (eliminación) o el fixture (liga).
- **Programar partidos**: fecha/hora.
- **Asignar canal de transmisión** a cada partido.

### Resultados (STAFF/CASTER/ADMIN) — núcleo operativo
- Lista de partidos `PROGRAMADO` / `EN_VIVO`, con estado de **check-in** de cada equipo.
- **Captura por mapa** (Best-of): por cada mapa → mapa + modo + ganador. Al alcanzar los
  mapas necesarios, la serie se marca `FINALIZADO`.
  - Guarda `reportadoPorId` y `canalTransmisionId`.
  - Dispara: avance de bracket / recálculo de tabla + notificaciones + webhook n8n.
- **Walkover:** el sistema lo marca automáticamente por no-show; el admin puede anular/ajustar.
- **Corrección:** solo `ADMIN` puede corregir una serie ya cerrada (sin flujo de disputa).

### Canales de transmisión (ADMIN)
- CRUD de canales de la liga (nombre, plataforma Twitch/YouTube, URL, activo).
- Un partido se asigna a un canal; un caster puede asociarse a un canal.

### Organizaciones (ADMIN)
- Ver marcas registradas y sus rosters.
- Moderación (renombrar/suspender en caso de abuso).

### Jugadores (ADMIN)
- **Ver todos los usuarios**; buscar/filtrar.
- **Detalle de usuario**: datos (nickname, gamertag, teléfono/verificación, fecha de
  nacimiento, país), **equipo actual y organización**, estado de **ventana de
  transferencia**, **redes sociales** y su **bitácora de conducta** (reportes + sanciones).

### Moderación (ADMIN)
- **Reportes**: bandeja de denuncias (`ABIERTO`/`EN_REVISION`) → revisar evidencia →
  resolver (aplicar sanción o descartar).
- **Sanciones**: emitir/anular sanción a un usuario (advertencia, suspensión temporal,
  expulsión de evento, baneo permanente) con motivo y severidad.
- Una sanción activa (suspensión/baneo) **bloquea** competir. Ver
  [13-cuenta-legal-y-moderacion.md](./13-cuenta-legal-y-moderacion.md).

### Staff (ADMIN)
- **Invitar por correo** a nuevos miembros de staff y asignar rol (`ADMIN` / `STAFF`).
- Gestionar/revocar accesos; asignar casters a canales.

### Auditoría (ADMIN)
- **Log de acciones sensibles** (capturar/corregir resultado, aprobar inscripción, mover
  fecha, emitir sanción) con autor y fecha.

### (v2) Contenido / IA · Reportes
- Generar **resúmenes de jornada** para redes.
- Reportes de participación, asistencia, etc.

## Roles y permisos (resumen)

| Acción | STAFF/CASTER | ADMIN |
|--------|:---:|:---:|
| Ver dashboard | ✅ | ✅ |
| Capturar resultados | ✅ | ✅ |
| Programar partidos / asignar canal | ✅ | ✅ |
| Crear/editar eventos | — | ✅ |
| Aprobar/rechazar inscripciones | — | ✅ |
| Gestionar canales / orgs / jugadores | — | ✅ |

## Notas de diseño
- Layout tipo panel: navegación lateral + tablas densas y formularios claros.
- Prioridad: que capturar un resultado sean **2-3 clics** (es la acción más repetida).
