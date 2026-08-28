# 04 · Especificación de API (v1)

API REST, JSON, prefijo `/api/v1`. Autenticación por **JWT** en header
`Authorization: Bearer <token>`. Roles: `PLAYER`, `ADMIN` (y `CAPITAN` a nivel equipo).

> Esta es la superficie **core del MVP**. No es exhaustiva; se refina al implementar.

## Convenciones

- IDs: UUID · Fechas: ISO-8601 UTC · Nombres: `camelCase`.
- Errores: `{ "code": "STRING", "message": "...", "details": {...} }`.
- Paginación (donde aplique): `?page=0&size=20`.

---

## 🔐 Auth (social + email; teléfono por SMS OTP)

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| POST | `/auth/oauth/google` | público | Login/registro con Google → JWT |
| POST | `/auth/oauth/apple` | público | Login/registro con Apple → JWT |
| POST | `/auth/oauth/xbox` | público | Login/registro con Xbox (trae gamertag) → JWT |
| POST | `/auth/register` | público | Registro con email + contraseña → JWT |
| POST | `/auth/login` | público | Login con email + contraseña → JWT |
| POST | `/auth/password/forgot` | público | Inicia recuperación de contraseña |
| GET | `/auth/me` | autenticado | Perfil del usuario actual (incluye `perfilCompleto`) |

## 👤 Perfil / Usuarios

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| PATCH | `/users/me` | autenticado | Completar/editar perfil (nickname, gamertag, fechaNacimiento, avatar, país) |
| POST | `/users/me/phone` | autenticado | Registrar teléfono → envía SMS OTP |
| POST | `/users/me/phone/verify` | autenticado | Verifica OTP del teléfono |
| POST | `/users/me/phone/resend` | autenticado | Reenvía OTP (rate-limited) |
| GET | `/users/me/socials` | autenticado | Listar mis redes sociales |
| PUT | `/users/me/socials` | autenticado | Reemplazar mis redes (plataforma + URL) |
| GET | `/users/:id` | autenticado | Ver perfil público (incluye redes sociales) |

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
| POST | `/teams/:id/invitations` | capitán | Crear invitación (código o por nickname; rol propuesto) |
| POST | `/invitations/:id/accept` | invitado | Aceptar invitación (valida 1-equipo + ventana 72h) |
| POST | `/invitations/:id/reject` | invitado | Rechazar invitación |
| DELETE | `/teams/:id/members/:userId` | capitán | Expulsar jugador (activa ventana 72h) |
| POST | `/teams/:id/transfer-captain` | capitán | Transferir capitanía a otro miembro |

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
| GET | `/matches/:id` | público | Detalle de un partido (incluye mapas de la serie) |
| PATCH | `/matches/:id/schedule` | admin | Programar fecha/hora + canal |
| POST | `/matches/:id/checkin` | capitán | Check-in de mi equipo (dentro de la ventana) |
| POST | `/matches/:id/maps` | staff | Registrar resultado de un mapa (mapa+modo+ganador) |
| PATCH | `/matches/:id/maps/:mapId` | staff | Corregir un mapa (solo admin corrige serie cerrada) |
| POST | `/matches/:id/reschedule-requests` | capitán | Solicitar reprogramación (motivo + fecha propuesta) |
| PATCH | `/matches/:id/reschedule-requests/:reqId` | admin | Aprobar (mueve fecha) o rechazar |
| POST | `/matches/:id/walkover` | admin | Marcar/anular walkover o doble forfeit |
| PATCH | `/matches/:id/result` | staff/admin | Cerrar/ajustar serie (admin corrige si ya cerró) |

## 📊 Standings (formatos de liga)

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| GET | `/events/:id/standings` | público | Tabla de posiciones calculada |

## 📦 Bracket (formatos de eliminación)

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| GET | `/events/:id/bracket` | público | Estructura de llaves con avances |

## 🚨 Moderación

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| POST | `/reports` | autenticado | Reportar conducta (reportado, motivo, evidencia) |
| GET | `/reports` | admin | Bandeja de reportes (filtro por estado) |
| PATCH | `/reports/:id` | admin | Resolver/descartar reporte |
| POST | `/users/:id/sanctions` | admin | Emitir sanción (tipo, severidad, motivo, vigencia) |
| GET | `/users/:id/sanctions` | admin | Bitácora de conducta del usuario |
| DELETE | `/sanctions/:id` | admin | Anular sanción |

## 🛠️ Staff / Auditoría

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| POST | `/staff/invitations` | admin | Invitar staff por correo + rol (ADMIN/STAFF) |
| GET | `/staff` | admin | Listar staff y sus roles |
| PATCH | `/staff/:userId` | admin | Cambiar rol / revocar acceso |
| GET | `/audit-log` | admin | Log de acciones sensibles |

---

## Notas de implementación

- `POST /events/:id/start` es el punto donde entra el **motor de formatos**
  (ver [ADR-0001](./adr/0001-motor-de-formatos.md)): valida inscripciones aprobadas,
  selecciona la estrategia por `formato` y genera los partidos.
- `PATCH /matches/:id/result` es transaccional: guarda marcador, marca `FINALIZADO` y,
  según formato, propaga ganador (eliminación) o deja que standings se recalcule (liga).
- Endpoints `public` no requieren token; el resto valida JWT y rol.
