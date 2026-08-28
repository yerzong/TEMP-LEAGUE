# 04 · Especificación de API (v1)

API REST, JSON, prefijo `/api/v1`. Autenticación por **JWT** en header
`Authorization: Bearer <token>`. Roles: `PLAYER`, `ADMIN` (y `CAPITAN` a nivel equipo).

> Esta es la superficie **core del MVP**. No es exhaustiva; se refina al implementar.

## Convenciones

- IDs: UUID · Fechas: ISO-8601 UTC · Nombres: `camelCase`.
- Errores: `{ "code": "STRING", "message": "...", "details": {...} }`.
- Paginación (donde aplique): `?page=0&size=20`.

---

## 🔐 Auth (teléfono + SMS OTP)

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| POST | `/auth/register` | público | Registro con teléfono → envía SMS OTP |
| POST | `/auth/otp/verify` | público | Verifica el código OTP → activa cuenta + JWT |
| POST | `/auth/otp/resend` | público | Reenvía OTP (rate-limited) |
| POST | `/auth/login` | público | Login (teléfono + contraseña) → JWT |
| GET | `/auth/me` | autenticado | Perfil del usuario actual |

## 👤 Usuarios

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| GET | `/users/:id` | autenticado | Ver perfil público de un jugador |
| PATCH | `/users/me` | autenticado | Editar nickname / gamertag / avatar / país |

## 🏢 Organizaciones

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| GET | `/orgs/check?nombre=&tag=` | autenticado | Validar unicidad de nombre/tag en vivo |
| POST | `/orgs` | autenticado | Crear organización (creador = owner) |
| GET | `/orgs/:id` | público | Detalle de org + sus equipos/rosters |
| PATCH | `/orgs/:id` | owner | Editar org (logo, descripción) |

## 🛡️ Equipos (rosters)

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| POST | `/orgs/:orgId/teams` | owner | Crear equipo/roster en la org |
| GET | `/teams/:id` | público | Detalle de equipo + roster |
| PATCH | `/teams/:id` | capitán/owner | Editar equipo |
| POST | `/teams/:id/members` | capitán | Invitar/agregar jugador (valida 1-equipo + ventana 72h) |
| DELETE | `/teams/:id/members/:userId` | capitán | Quitar jugador (activa ventana 72h) |

## 🏆 Eventos

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| GET | `/events` | público | Listar eventos (filtro por `tipo`, `estado`) |
| GET | `/events/:id` | público | Detalle: info, formato, equipos inscritos |
| POST | `/events` | admin | Crear evento (tipo, formato, fechas, maxEquipos) |
| PATCH | `/events/:id` | admin | Editar evento (mientras esté en BORRADOR/INSCRIPCIONES) |
| POST | `/events/:id/open` | admin | Abrir inscripciones (`BORRADOR → INSCRIPCIONES`) |
| POST | `/events/:id/start` | admin | Iniciar: genera bracket/tabla (`INSCRIPCIONES → EN_CURSO`) |
| POST | `/events/:id/finish` | admin | Cerrar evento (`EN_CURSO → FINALIZADO`) |

## 📝 Inscripciones

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| POST | `/events/:id/registrations` | capitán | Inscribir su equipo al evento |
| GET | `/events/:id/registrations` | público | Ver equipos inscritos y su estado |
| PATCH | `/events/:id/registrations/:regId` | admin | Aprobar/rechazar/asignar seed |
| DELETE | `/events/:id/registrations/:regId` | capitán/admin | Retirar inscripción |

## ⚔️ Partidos

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| GET | `/events/:id/matches` | público | Todos los partidos del evento (bracket/calendario) |
| GET | `/matches/:id` | público | Detalle de un partido |
| PATCH | `/matches/:id/schedule` | admin | Programar fecha/hora |
| PATCH | `/matches/:id/result` | admin | Capturar marcador → propaga ganador / recalcula tabla |

## 📊 Standings (formatos de liga)

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| GET | `/events/:id/standings` | público | Tabla de posiciones calculada |

## 📦 Bracket (formatos de eliminación)

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| GET | `/events/:id/bracket` | público | Estructura de llaves con avances |

---

## Notas de implementación

- `POST /events/:id/start` es el punto donde entra el **motor de formatos**
  (ver [ADR-0001](./adr/0001-motor-de-formatos.md)): valida inscripciones aprobadas,
  selecciona la estrategia por `formato` y genera los partidos.
- `PATCH /matches/:id/result` es transaccional: guarda marcador, marca `FINALIZADO` y,
  según formato, propaga ganador (eliminación) o deja que standings se recalcule (liga).
- Endpoints `public` no requieren token; el resto valida JWT y rol.
