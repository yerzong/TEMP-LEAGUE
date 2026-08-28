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
| email | string | **único**; de login social o registro email/contraseña |
| emailVerificado | bool | true si el proveedor social lo verifica o por confirmación |
| passwordHash | string? | solo si usó registro email + contraseña |
| authProviders | enum[] | proveedores vinculados: `GOOGLE`, `APPLE`, `XBOX`, `PASSWORD` |
| nickname | string | nombre visible |
| gamertag | string? | autollenado si entró con Xbox; **requerido para competir** |
| telefono | string? | **único**; **requerido y verificado** para competir; contacto/WhatsApp |
| telefonoVerificado | bool | true tras validar SMS OTP |
| fechaNacimiento | date? | **requerido para competir**; se guarda fecha (no edad); **sin restricción de edad** |
| avatarUrl | string? | foto de perfil (opcional) |
| pais | string | LATAM |
| perfilCompleto | bool | true cuando tiene gamertag + teléfono verificado + fechaNacimiento |
| rolGlobal | enum | `PLAYER`, `STAFF`, `ADMIN` |
| transferLockUntil | timestamp? | fin de la ventana de transferencia (72 h) |
| creadoEn | timestamp | — |

> **Roles no excluyentes:** un mismo usuario puede ser **dueño de organización** (nivel org),
> **capitán/jugador** de un equipo (nivel equipo, máx. 1 activo) y tener un **rol global**
> a la vez. Ser dueño **no** consume el cupo de "1 equipo activo"; jugar sí. Ver
> [reglas de negocio](./09-reglas-negocio.md).

### RedSocialUsuario (perfil, opcional)
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| usuarioId | UUID | FK → Usuario |
| plataforma | enum | `TWITTER`, `FACEBOOK`, `INSTAGRAM`, `TWITCH`, `YOUTUBE`, `TIKTOK`, `OTRA` |
| url | string | URL o usuario de la red |

> El usuario elige la plataforma de un listado y pega su URL/usuario. Varias, todas
> opcionales. Visibles en el detalle de usuario del Admin Web.

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
| premio | decimal? | bolsa/premio a mostrar (opcional) |
| reglamento | text? | reglas del evento (texto o URL), visible en la app |
| maxEquipos | int | cupo |
| **— Config de roster —** | | *(por evento, roster configurable)* |
| rosterMin | int | mínimo de titulares (ej. 5) |
| rosterMax | int | máximo de titulares |
| maxSuplentes | int | banca permitida |
| permiteCoach | bool | permite coach/manager |
| **— Config de partido —** | | |
| bestOf | int | 1, 3, 5, 7 (mapas por serie) |
| modosDisponibles | string[] | modos permitidos (lista editable: Ejecución, Rey de la Colina, Control…) |
| **— Config de día de partido —** | | |
| checkinAbreMin | int | minutos antes en que abre el check-in (ej. 30) |
| walkoverTrasMin | int | minutos de tolerancia antes de walkover (ej. 15) |
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
| marcadorLocal | int? | mapas ganados por local (derivado de PartidoMapa) |
| marcadorVisitante | int? | mapas ganados por visitante (derivado) |
| estado | enum | `PROGRAMADO`, `EN_VIVO`, `FINALIZADO` |
| resultadoTipo | enum | `NORMAL`, `WALKOVER`, `DOBLE_FORFEIT` |
| checkinLocal | timestamp? | cuándo hizo check-in el equipo local |
| checkinVisitante | timestamp? | cuándo hizo check-in el visitante |
| siguientePartidoId | UUID? | avance del ganador (solo eliminación) |
| reportadoPorId | UUID? | staff/caster/admin que capturó el resultado |
| canalTransmisionId | UUID? | canal donde se transmite |

### PartidoMapa (mapa dentro de la serie Best-of)
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| partidoId | UUID | FK → Partido |
| orden | int | número de mapa en la serie (1..bestOf) |
| mapa | string | nombre del mapa |
| modo | string | modo jugado (de `modosDisponibles` del evento) |
| ganadorEquipoId | UUID? | equipo que ganó el mapa |
| marcadorLocal | int? | marcador del mapa (opcional) |
| marcadorVisitante | int? | — |

> El marcador de serie (`Partido.marcadorLocal/Visitante`) = conteo de mapas ganados.
> La serie termina cuando un equipo alcanza `ceil(bestOf/2)` mapas.

### SolicitudReprogramacion
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| partidoId | UUID | FK → Partido |
| solicitanteEquipoId | UUID | FK → Equipo (quién pide) |
| motivo | string | razón de la reprogramación |
| fechaPropuesta | timestamp? | nueva fecha sugerida (opcional) |
| estado | enum | `PENDIENTE`, `APROBADA`, `RECHAZADA` |
| resueltaPorId | UUID? | admin que resolvió |
| creadoEn | timestamp | — |

> Solo el **admin** aprueba y mueve `Partido.fechaProgramada`. Ver
> [12-formato-partido-y-dia-de-partido.md](./12-formato-partido-y-dia-de-partido.md).

### InvitacionEquipo
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| equipoId | UUID | FK → Equipo |
| codigo | string? | código de invitación (alternativa a invitar por nickname) |
| usuarioId | UUID? | invitado directo (si aplica) |
| rolPropuesto | enum | `JUGADOR`, `SUPLENTE`, `COACH` |
| estado | enum | `PENDIENTE`, `ACEPTADA`, `RECHAZADA`, `EXPIRADA` |
| creadoEn | timestamp | — |

> El invitado **acepta** para unirse (respetando "1 equipo activo" + ventana 72 h).

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

### Sancion (bitácora de conducta)
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| usuarioId | UUID | FK → Usuario sancionado |
| tipo | enum | `ADVERTENCIA`, `SUSPENSION_TEMPORAL`, `EXPULSION_EVENTO`, `BANEO_PERMANENTE` |
| severidad | enum | `LEVE`, `MEDIA`, `GRAVE` |
| motivo | text | descripción de la falta |
| eventoId | UUID? | si la sanción aplica a un evento |
| emitidoPorId | UUID | FK → Usuario (admin) |
| fechaInicio | timestamp | — |
| fechaFin | timestamp? | para suspensiones temporales |
| activa | bool | derivada de tipo/fechaFin |

### Reporte (denuncia de conducta)
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| reportanteId | UUID | FK → Usuario que reporta |
| reportadoId | UUID | FK → Usuario reportado |
| partidoId | UUID? | contexto (opcional) |
| motivo | string | categoría de la falta |
| descripcion | text? | detalle |
| evidenciaUrl | string? | screenshot/clip (opcional) |
| estado | enum | `ABIERTO`, `EN_REVISION`, `RESUELTO`, `DESCARTADO` |
| resueltoPorId | UUID? | admin que resolvió |
| creadoEn | timestamp | — |

### RegistroAuditoria (acciones sensibles del staff)
| Campo | Tipo | Notas |
|-------|------|-------|
| id | UUID | PK |
| actorId | UUID | staff/admin que ejecutó |
| accion | string | ej. "CAPTURAR_RESULTADO", "APROBAR_INSCRIPCION", "EMITIR_SANCION" |
| entidad | string | tipo + id afectado |
| detalle | json? | antes/después |
| creadoEn | timestamp | — |

## Enums (contratos compartidos — `packages/shared`)

```
RolGlobal        = PLAYER | STAFF | ADMIN
RolEquipo        = CAPITAN | JUGADOR | SUPLENTE | COACH
ResultadoTipo    = NORMAL | WALKOVER | DOBLE_FORFEIT
EstadoInvitacion = PENDIENTE | ACEPTADA | RECHAZADA | EXPIRADA
TipoSancion      = ADVERTENCIA | SUSPENSION_TEMPORAL | EXPULSION_EVENTO | BANEO_PERMANENTE
SeveridadSancion = LEVE | MEDIA | GRAVE
EstadoReporte    = ABIERTO | EN_REVISION | RESUELTO | DESCARTADO
AuthProvider     = GOOGLE | APPLE | XBOX | PASSWORD
RedSocialPlataf. = TWITTER | FACEBOOK | INSTAGRAM | TWITCH | YOUTUBE | TIKTOK | OTRA
TipoEvento       = TEMPORADA | RELAMPAGO
FormatoEvento    = SINGLE_ELIM | DOUBLE_ELIM | ROUND_ROBIN | LEAGUE_PLAYOFFS
EstadoEvento     = BORRADOR | INSCRIPCIONES | EN_CURSO | FINALIZADO
EstadoInscrip.   = PENDIENTE | APROBADA | RECHAZADA
PagoEstado       = NO_APLICA | PENDIENTE | PAGADO | FALLIDO | REEMBOLSADO
EstadoPartido    = PROGRAMADO | EN_VIVO | FINALIZADO
Plataforma       = TWITCH | YOUTUBE   (canales de transmisión)
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
