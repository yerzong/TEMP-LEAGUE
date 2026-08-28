# 12 · Formato de partido y día de partido

Cómo se compone un equipo, cómo se juega y captura un partido, y qué pasa el día del
enfrentamiento. Todo **configurable por evento** para adaptarse a la meta de Gears E-Day
(que aún no está fija al ser un juego nuevo).

## Roster (configurable por evento)

Cada evento define su configuración de roster:
- `rosterMin` / `rosterMax` — titulares (Gears es **5v5**, así que típico `5`).
- `maxSuplentes` — banca permitida.
- `permiteCoach` — si admite coach/manager.

Roles dentro del equipo (`RolEquipo`): `CAPITAN`, `JUGADOR`, `SUPLENTE`, `COACH`.

> Se valida al inscribir: el roster debe cumplir el mínimo de titulares del evento.

## Formato de partido — Best-of con captura por mapa

- Cada evento define su **`bestOf`** (1, 3, 5, 7) y su lista **`modosDisponibles`**
  (editable: Ejecución, Rey de la Colina, Control… se ajusta cuando E-Day asiente su meta).
- Un **Partido** es una **serie**; cada mapa es un **`PartidoMapa`** con: orden, mapa,
  modo y ganador.
- El **marcador de la serie** = conteo de mapas ganados. La serie termina cuando un equipo
  llega a `ceil(bestOf / 2)` (ej. 2 en BO3, 3 en BO5).

### Captura (desde el Admin Web, por staff/caster)
```
Staff abre el partido EN_VIVO
  → registra mapa 1: mapa + modo + ganador
  → registra mapa 2 …
  → al alcanzar los mapas necesarios → Partido FINALIZADO (marcador de serie)
  → avanza bracket / recalcula tabla
```

## Día de partido — check-in + walkover automático

### Check-in
- Se abre una ventana **`checkinAbreMin`** minutos antes (ej. 30).
- Cada equipo (capitán) hace **check-in** desde la app → `checkinLocal` / `checkinVisitante`.

### Walkover (no-show)
- Si a la hora del partido + **`walkoverTrasMin`** (ej. 15) un equipo **no hizo check-in**
  o no se presenta:
  - El sistema marca `resultadoTipo = WALKOVER` → **victoria por default** al rival.
  - Si **ninguno** se presenta → `DOBLE_FORFEIT`.
- Es **automático**: no requiere que un admin persiga a nadie (aunque el admin puede
  anular/ajustar si hubo causa justificada).

## Resultados — staff final, sin disputa

- El resultado lo captura **staff/caster/admin** desde el Admin Web (fuente oficial única).
- **No hay flujo de disputa** por parte de los equipos: lo capturado es **definitivo**.
- Un **admin** puede **corregir** un resultado si hubo un error de captura (queda registrado
  `reportadoPorId` y, en v2, log de auditoría).

## Invitaciones y vida del equipo

- **Invitar** por **código de invitación** o por **búsqueda de nickname**; el invitado
  **acepta** para unirse (respeta "1 equipo activo" + ventana 72 h).
- Rol al invitar: `JUGADOR`, `SUPLENTE` o `COACH`.
- El **capitán** puede **expulsar** miembros y **transferir la capitanía**.
- Si el capitán sale, debe **transferir la capitanía** antes; si no, el equipo queda
  inactivo hasta reasignar capitán (política a afinar en implementación).

## Invariantes (para tests)

- [ ] Roster cumple `rosterMin` de titulares al inscribirse.
- [ ] La serie termina exactamente al alcanzar `ceil(bestOf/2)` mapas.
- [ ] `marcadorLocal/Visitante` del partido = mapas ganados (derivado, consistente).
- [ ] Walkover solo si venció `walkoverTrasMin` sin check-in/presencia.
- [ ] Resultado capturado solo por `STAFF`/`ADMIN`; corrección solo por `ADMIN`.
- [ ] Aceptar invitación respeta "1 equipo activo" y ventana 72 h.
