# 03 · Modelo de datos

El diseño gira en torno a un concepto raíz flexible: el **Evento**. Un mismo motor
soporta Temporadas y Relámpagos cambiando el `formato`.

## Diagrama entidad-relación (conceptual)

```
┌─────────────┐        ┌──────────────┐        ┌─────────────┐
│   Usuario   │◄──────►│  MiembroDe   │◄──────►│   Equipo    │
│             │        │  (rol equipo)│        │             │
│ id          │        └──────────────┘        │ id          │
│ email       │                                │ nombre      │
│ nickname    │                                │ tag         │
│ rolGlobal   │                                │ capitanId   │
└─────────────┘                                └──────┬──────┘
                                                      │
                                                      │ (via Inscripción)
                                                      ▼
┌───────────────────────────┐              ┌──────────────────┐
│         Evento            │◄─────────────│   Inscripción    │
│                           │  1        N  │                  │
│ id                        │              │ id               │
│ nombre                    │              │ eventoId         │
│ tipo:  TEMPORADA|RELAMPAGO│              │ equipoId         │
│ formato: (ver enum)       │              │ estado           │
│ estado: (ver enum)        │              │ seed             │
│ fechaInicio / fechaFin    │              └──────────────────┘
│ maxEquipos                │
└─────────────┬─────────────┘
              │ 1
              │ genera
              │ N
              ▼
┌───────────────────────────┐
│         Partido           │
│                           │
│ id                        │
│ eventoId                  │
│ equipoLocalId             │
│ equipoVisitanteId         │
│ ronda / jornada           │
│ fechaProgramada           │
│ marcadorLocal / Visitante │
│ estado: (ver enum)        │
│ siguientePartidoId (elim.)│
└───────────────────────────┘
```

## Entidades

### Usuario
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| email | string | único |
| passwordHash | string | — |
| nickname | string | nombre en juego / gamertag |
| rolGlobal | enum | `PLAYER`, `ADMIN` |
| creadoEn | timestamp | — |

### Equipo
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| nombre | string | — |
| tag | string | abreviatura, ej. "TMP" |
| logoUrl | string? | opcional |
| capitanId | UUID | FK → Usuario |
| creadoEn | timestamp | — |

### MiembroDe (Usuario ↔ Equipo)
| Campo | Tipo | Notas |
|-------|------|-------|
| usuarioId | UUID | FK |
| equipoId | UUID | FK |
| rolEquipo | enum | `CAPITAN`, `JUGADOR` |

### Evento
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| nombre | string | ej. "Temp League Temporada 1" |
| tipo | enum | `TEMPORADA`, `RELAMPAGO` |
| formato | enum | `SINGLE_ELIM`, `DOUBLE_ELIM`, `ROUND_ROBIN`, `LEAGUE_PLAYOFFS` |
| estado | enum | `BORRADOR`, `INSCRIPCIONES`, `EN_CURSO`, `FINALIZADO` |
| fechaInicio | date | — |
| fechaFin | date? | — |
| maxEquipos | int | límite de inscripción |
| creadoEn | timestamp | — |

### Inscripción (Equipo ↔ Evento)
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| eventoId | UUID | FK |
| equipoId | UUID | FK |
| estado | enum | `PENDIENTE`, `APROBADA`, `RECHAZADA` |
| seed | int? | siembra para el bracket (asignada por admin) |
| creadoEn | timestamp | — |

> El **roster** por evento se deriva de los miembros del equipo al momento de inscribir
> (en el MVP se toma el equipo completo; en v2 se podrá fijar roster por evento).

### Partido
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| eventoId | UUID | FK |
| equipoLocalId | UUID? | null hasta conocerse (bracket) |
| equipoVisitanteId | UUID? | null hasta conocerse |
| ronda | int | ronda (elim.) o jornada (liga) |
| fechaProgramada | timestamp? | — |
| marcadorLocal | int? | — |
| marcadorVisitante | int? | — |
| estado | enum | `PROGRAMADO`, `EN_VIVO`, `FINALIZADO` |
| siguientePartidoId | UUID? | a dónde avanza el ganador (solo eliminación) |

## Enums (contratos compartidos)

Estos enums viven en `packages/shared` y se replican en backend, app y admin.

```
TipoEvento     = TEMPORADA | RELAMPAGO
FormatoEvento  = SINGLE_ELIM | DOUBLE_ELIM | ROUND_ROBIN | LEAGUE_PLAYOFFS
EstadoEvento   = BORRADOR | INSCRIPCIONES | EN_CURSO | FINALIZADO
EstadoInscrip. = PENDIENTE | APROBADA | RECHAZADA
EstadoPartido  = PROGRAMADO | EN_VIVO | FINALIZADO
RolGlobal      = PLAYER | ADMIN
RolEquipo      = CAPITAN | JUGADOR
```

## Reglas de negocio clave

1. Un evento solo genera partidos cuando pasa de `INSCRIPCIONES` a `EN_CURSO`.
2. Solo inscripciones `APROBADA` entran a la generación de bracket/tabla.
3. En eliminación, cerrar un partido (`FINALIZADO`) propaga el ganador a
   `siguientePartidoId`.
4. En liga (round-robin), la tabla de posiciones se calcula a partir de los partidos
   `FINALIZADO` (victoria/derrota/diferencia).
5. `maxEquipos` debe ser potencia de 2 para brackets de eliminación limpia (o se manejan
   *byes*).

## Tabla de posiciones (derivada, no almacenada en MVP)

Para formatos de liga se calcula on-the-fly a partir de los partidos:
`puntos`, `victorias`, `derrotas`, `diferencia de rondas`. Se puede materializar/cachear
en v2 si hace falta rendimiento.
