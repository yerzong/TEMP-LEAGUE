# 02 · Arquitectura y stack

## Diagrama de sistema

```
        ┌─────────────────────┐         ┌─────────────────────┐
        │   App Jugadores     │         │   Consola Admin     │
        │   Flutter (móvil)   │         │   React (web)       │
        │  iOS · Android      │         │                     │
        └──────────┬──────────┘         └──────────┬──────────┘
                   │                                │
                   │        HTTPS / REST (JSON)     │
                   └────────────────┬───────────────┘
                                    │
                          ┌─────────▼─────────┐
                          │   API Backend     │
                          │   Spring Boot     │
                          │  (Java 21, REST)  │
                          │                   │
                          │  Auth · Eventos   │
                          │  Equipos · Match  │
                          │  Motor de formato │
                          └─────────┬─────────┘
                                    │  JDBC
                          ┌─────────▼─────────┐
                          │   PostgreSQL      │
                          └───────────────────┘
```

Ambos clientes (app y admin) consumen **la misma API REST**. El backend es la única
fuente de verdad y contiene toda la lógica de negocio (incluido el *motor de formatos*
que genera brackets y tablas).

## Stack tecnológico

| Capa | Tecnología | Por qué |
|------|-----------|---------|
| **Backend** | Java 21 + Spring Boot | Robusto, tipado, terreno conocido; ideal para lógica relacional de ligas |
| **Base de datos** | PostgreSQL | Relacional (equipos↔eventos↔partidos), maduro, gratis |
| **App móvil** | Flutter (Dart) | Un solo código → iOS + Android; UI pulida; opción de build web |
| **Admin web** | React + TypeScript | El admin es data-heavy (tablas/formularios); React + librería de UI vuela |
| **Contratos** | `packages/shared` | Enums/DTOs compartidos para no desincronizar cliente y servidor |
| **Infra** | Railway / Render (candidato) | Deploy simple de backend + Postgres administrado |

## Decisiones de arquitectura

### 1. Monorepo
Todo (backend, app, admin, docs) en un solo repo, separado por carpetas. Facilita
mantener contratos sincronizados y da contexto completo. Ver [estructura](./06-estructura-monorepo.md).

### 2. Motor de formatos en el backend
En lugar de programar cada tipo de torneo por separado, el backend tiene un **motor de
formatos**: dado un evento con `formato = SINGLE_ELIM | DOUBLE_ELIM | ROUND_ROBIN |
LEAGUE_PLAYOFFS`, genera automáticamente partidos, rondas/jornadas y estructura. Agregar
un formato nuevo = una estrategia nueva, sin tocar el resto. Ver [ADR-0001](./adr/0001-motor-de-formatos.md).

### 3. API REST (no GraphQL) para el MVP
Menos ceremonia, más velocidad de entrega. Suficiente para el alcance actual.

### 4. Auth con JWT
Login → token JWT. El backend valida rol (`PLAYER`, `CAPTAIN`, `ADMIN`) por endpoint.

### 5. Estrategia de lanzamiento móvil
Decisión: **app nativa primero**. Riesgo principal = revisión de App Store (Apple).
Mitigación: subir a revisión ~2 semanas antes del 6 oct y correr beta con la comunidad
vía **TestFlight (iOS)** y **Play Internal Testing (Android)**. Ver [roadmap](./05-roadmap.md).

## Estándares técnicos (a definir en detalle)

- **Formato de API:** JSON, convención `camelCase`.
- **Errores:** estructura estándar `{ code, message, details }`.
- **Versionado:** prefijo `/api/v1`.
- **Fechas:** ISO-8601 UTC.
- **IDs:** UUID.

## Diagrama de despliegue (objetivo)

```
Apple App Store  ◄── Flutter build (iOS)
Google Play      ◄── Flutter build (Android)
Vercel/Netlify   ◄── React admin (web estático)
Railway/Render   ◄── Spring Boot + PostgreSQL administrado
```
