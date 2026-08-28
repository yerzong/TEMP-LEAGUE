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
     ├─ Jugadores          (usuarios, moderación)
     └─ (v2) Contenido/IA  (resúmenes, posts) · Reportes
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
- Lista de partidos `PROGRAMADO` / `EN_VIVO`.
- **Capturar marcador** → marcar `FINALIZADO`.
  - Guarda `reportadoPorId` (quién capturó) y `canalTransmisionId`.
  - Dispara: avance de bracket / recálculo de tabla + notificaciones + webhook n8n.
- (Opción futura) marcar `EN_VIVO` al iniciar transmisión.

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
  transferencia**, y sus **redes sociales**.
- Moderar (suspender, ajustar rol global).

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
