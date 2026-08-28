# 03 · Modelo de datos

Diseño centrado en dos ideas:
1. **Organización → Equipos/Rosters** (una marca puede tener varias alineaciones).
2. Un **Evento** flexible cuyo `formato` define cómo se compite (motor de formatos).

## Diagrama entidad-relación (conceptual)

```
┌─────────────┐   owner    ┌────────────────┐  1   N ┌──────────────┐
│   Usuario   │───────────►│  Organización  │───────►│    Equipo    │
│             │            │  (marca única) │        │  (Roster)    │
│ id          │            │ id             │        │ id           │
│ telefono ✔  │            │ nombre (uniq)  │        │ orgId        │
│ nickname    │            │ tag (uniq)     │        │ nombre       │
│ gamertag    │            │ logoUrl        │        │ capitanId    │
│ rolGlobal   │            │ ownerId        │        └──────┬───────┘
│ transferLock│            └────────────────┘               │
└──────┬──────┘                                             │ miembros
       │            ┌──────────────────────┐                │
       └───────────►│   MiembroDeEquipo    │◄───────────────┘
        (1 activo)  │ usuarioId · equipoId │
                    │ rolEquipo · joinedAt │
                    └──────────────────────┘

┌───────────────────────────┐          ┌──────────────────────┐
│         Evento            │◄─────────│     Inscripción      │
│ id                        │  1    N  │ id · eventoId        │
│ nombre                    │          │ equipoId (roster)    │
│ tipo: TEMPORADA|RELAMPAGO │          │ estado               │
│ formato                   │          │ seed                 │
│ estado                    │          │ pagoEstado           │
│ cuotaEntrada (0 = gratis) │          └──────────────────────┘
│ moneda                    │
│ maxEquipos                │
└─────────────┬─────────────┘
              │ 1 → N (motor de formatos)
              ▼
┌───────────────────────────┐          ┌──────────────────────┐
│         Partido           │─────────►│  CanalTransmisión    │
│ id · eventoId             │  N    1  │ id · nombre          │
│ equipoLocalId / VisitId   │          │ plataforma · url     │
│ ronda / jornada           │          └──────────────────────┘
│ fechaProgramada           │
│ marcadorLocal / Visitante │
│ estado                    │
│ siguientePartidoId (elim) │
│ reportadoPorId (staff)    │
│ canalTransmisionId        │
└───────────────────────────┘
```

## Entidades

### Usuario
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| telefono | string | **único**; verificado por SMS OTP |
| telefonoVerificado | bool | true tras validar OTP |
| email | string? | opcional |
| passwordHash | string | — |
| nickname | string | nombre visible |
| gamertag | string? | gamertag de Xbox (texto, MVP no valida con Xbox Live) |
| avatarUrl | string? | foto de perfil |
| pais | string | LATAM |
| rolGlobal | enum | `PLAYER`, `STAFF`, `ADMIN` |
| transferLockUntil | timestamp? | fin de la ventana de transferencia (72 h) |
| creadoEn | timestamp | — |

### Organización (marca)
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| nombre | string | **único** (case-insensitive) |
| tag | string | **único**, abreviatura (ej. "ZRC") |
| logoUrl | string? | — |
| descripcion | string? | — |
| ownerId | UUID | FK → Usuario (dueño) |
| creadoEn | timestamp | — |

### Equipo (Roster)
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| orgId | UUID | FK → Organización |
| nombre | string | ej. "Main", "Academy"; único dentro de la org |
| capitanId | UUID | FK → Usuario |
| creadoEn | timestamp | — |

### MiembroDeEquipo (Usuario ↔ Equipo)
| Campo | Tipo | Notas |
|-------|------|-------|
| usuarioId | UUID | FK |
| equipoId | UUID | FK |
| rolEquipo | enum | `CAPITAN`, `JUGADOR` |
| joinedAt | timestamp | — |

> **Regla:** un usuario tiene **como máximo un MiembroDeEquipo activo** a la vez
> (ver [reglas de negocio](./09-reglas-negocio.md)).

### Evento
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| nombre | string | — |
| tipo | enum | `TEMPORADA`, `RELAMPAGO` |
| formato | enum | `SINGLE_ELIM`, `DOUBLE_ELIM`, `ROUND_ROBIN`, `LEAGUE_PLAYOFFS` |
| estado | enum | `BORRADOR`, `INSCRIPCIONES`, `EN_CURSO`, `FINALIZADO` |
| cuotaEntrada | decimal | `0` = gratis; > 0 = requiere pago |
| moneda | string | ej. "MXN" (aplica si hay cuota) |
| maxEquipos | int | cupo |
| creadoEn | timestamp | — |

### Inscripción (Equipo ↔ Evento)
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| eventoId | UUID | FK |
| equipoId | UUID | FK (el roster inscrito) |
| estado | enum | `PENDIENTE`, `APROBADA`, `RECHAZADA` |
| pagoEstado | enum | `NO_APLICA`, `PENDIENTE`, `PAGADO` |
| seed | int? | siembra para el bracket |
| creadoEn | timestamp | — |

### Partido
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| eventoId | UUID | FK |
| equipoLocalId | UUID? | null hasta conocerse (bracket) |
| equipoVisitanteId | UUID? | null hasta conocerse |
| ronda | int | ronda (elim.) / jornada (liga) |
| fechaProgramada | timestamp? | — |
| marcadorLocal | int? | — |
| marcadorVisitante | int? | — |
| estado | enum | `PROGRAMADO`, `EN_VIVO`, `FINALIZADO` |
| siguientePartidoId | UUID? | avance del ganador (solo eliminación) |
| reportadoPorId | UUID? | staff/caster/admin que capturó el resultado |
| canalTransmisionId | UUID? | canal donde se transmite |

### CanalTransmisión
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| nombre | string | ej. "Temp League TV" |
| plataforma | enum | `TWITCH`, `YOUTUBE` |
| url | string | — |
| activo | bool | — |

### Pago *(v2, cuando se cobre entrada)*
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| inscripcionId | UUID | FK |
| monto | decimal | — |
| proveedor | enum | `MERCADOPAGO`, `STRIPE` |
| referencia | string | id de la transacción externa |
| estado | enum | `PENDIENTE`, `PAGADO`, `FALLIDO`, `REEMBOLSADO` |

## Enums (contratos compartidos — `packages/shared`)

```
RolGlobal      = PLAYER | STAFF | ADMIN
RolEquipo      = CAPITAN | JUGADOR
TipoEvento     = TEMPORADA | RELAMPAGO
FormatoEvento  = SINGLE_ELIM | DOUBLE_ELIM | ROUND_ROBIN | LEAGUE_PLAYOFFS
EstadoEvento   = BORRADOR | INSCRIPCIONES | EN_CURSO | FINALIZADO
EstadoInscrip. = PENDIENTE | APROBADA | RECHAZADA
PagoEstado     = NO_APLICA | PENDIENTE | PAGADO | FALLIDO | REEMBOLSADO
EstadoPartido  = PROGRAMADO | EN_VIVO | FINALIZADO
Plataforma     = TWITCH | YOUTUBE
```

## Reglas de integridad clave

1. `Organizacion.nombre` y `Organizacion.tag` son **únicos** (validación automática,
   case-insensitive).
2. Un usuario tiene **máximo un equipo activo**; para cambiar, respeta la **ventana de 72 h**.
3. Solo inscripciones `APROBADA` (y `PAGADO` si hay cuota) entran a la generación de
   bracket/tabla.
4. El resultado de un partido solo lo captura un usuario `STAFF`/`ADMIN` (desde Admin Web);
   se guarda en `reportadoPorId`.
5. En eliminación, cerrar un partido propaga el ganador a `siguientePartidoId`.
6. La **tabla de posiciones** en formatos de liga se calcula on-the-fly desde los partidos
   `FINALIZADO` (v2: materializar si hace falta).

Detalle completo de reglas y flujos en [09-reglas-negocio.md](./09-reglas-negocio.md).
