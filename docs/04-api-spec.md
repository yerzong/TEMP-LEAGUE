# 04 · Especificación de API (v1)

API REST, JSON, prefijo `/api/v1`. Autenticación por **JWT** en header
`Authorization: Bearer <token>`. Roles: `PLAYER`, `ADMIN` (y `CAPITAN` a nivel equipo).

> Esta es la superficie **core del MVP**. No es exhaustiva; se refina al implementar.

## Convenciones

- IDs: UUID · Fechas: ISO-8601 UTC · Nombres: `camelCase`.
- Errores: `{ "code": "STRING", "message": "...", "details": {...} }`.
- Paginación (donde aplique): `?page=0&size=20`.

---

## 🔐 Auth

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| POST | `/auth/register` | público | Registro de jugador |
| POST | `/auth/login` | público | Login → devuelve JWT |
| GET | `/auth/me` | autenticado | Perfil del usuario actual |

## 👤 Usuarios

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| GET | `/users/:id` | autenticado | Ver perfil público de un jugador |
| PATCH | `/users/me` | autenticado | Editar nickname / datos propios |

## 🛡️ Equipos

| Método | Ruta | Rol | Descripción |
|--------|------|-----|-------------|
| POST | `/teams` | autenticado | Crear equipo (el creador queda como CAPITAN) |
| GET | `/teams/:id` | público | Detalle de equipo + roster |
| PATCH | `/teams/:id` | capitán | Editar equipo (nombre, tag, logo) |
| POST | `/teams/:id/members` | capitán | Invitar/agregar jugador |
| DELETE | `/teams/:id/members/:userId` | capitán | Quitar jugador |

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
