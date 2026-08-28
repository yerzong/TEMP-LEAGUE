# 02 · Arquitectura y stack

## Diagrama de sistema

```
     ┌──────────────────────┐        ┌──────────────────────────┐
     │   App Jugadores      │        │   Consola Admin/Staff    │
     │   React Native (Expo)│        │   React (web)            │
     │   iOS · Android      │        │                          │
     │   — CONSUMO (ver) —  │        │  — OPERACIÓN (capturar) —│
     └──────────┬───────────┘        └────────────┬─────────────┘
                │                                  │
                │       HTTPS / REST (JSON, JWT)   │
                └───────────────┬──────────────────┘
                                │
                      ┌─────────▼──────────┐        ┌──────────────┐
                      │   API Backend      │◄──────►│   n8n        │
                      │   NestJS (TS)      │webhooks│ (automatizar)│
                      │                    │        └──────┬───────┘
                      │ Auth(SMS OTP)·Orgs │               │
                      │ Equipos·Eventos    │        Discord·Redes·Email
                      │ Motor de formatos  │
                      │ Partidos·Resultados│        ┌──────────────┐
                      └───┬────────────┬───┘        │  FCM (push)  │
                          │ Prisma     │───────────►│              │
                  ┌───────▼──────┐ ┌───▼──────┐     └──────────────┘
                  │ PostgreSQL   │ │  Redis   │
                  └──────────────┘ │ (colas,  │
                                   │  OTP TTL)│
                                   └──────────┘
```

**Idea clave del reparto de responsabilidades:**
- **App móvil (React Native)** = experiencia de **consumo** del jugador: ver eventos,
  brackets, tablas, sus partidos, gestionar su equipo/inscripción.
- **Admin Web (React)** = herramienta de **operación** de la liga: crear eventos, aprobar,
  **capturar resultados** (staff/caster/admin), asignar canales de transmisión.
- El **backend NestJS** es la única fuente de verdad y contiene la lógica (incluido el
  *motor de formatos*).

## Stack tecnológico

| Capa | Tecnología | Por qué |
|------|-----------|---------|
| **Backend** | NestJS (Node + TypeScript) | Modular, escalable, mantenible; mismo lenguaje que los clientes |
| **ORM** | Prisma | Type-safe, migraciones claras, gran DX con Postgres |
| **Base de datos** | PostgreSQL | Relacional (org↔equipos↔eventos↔partidos), maduro |
| **Cache / colas / OTP** | Redis | TTL de códigos OTP, colas de notificaciones, rate-limiting |
| **App móvil** | React Native (Expo) | iOS + Android desde un código; ecosistema maduro |
| **Admin web** | React + TypeScript | Data-heavy (tablas/formularios); rápido de construir |
| **Contratos** | `packages/shared` | Tipos/DTOs/enums TS compartidos por todo el stack |
| **Push** | Firebase Cloud Messaging | Notificaciones push a la app |
| **SMS OTP** | Proveedor SMS (Twilio / MessageBird) | Verificación de teléfono al registrarse |
| **Automatización** | n8n | Webhooks del backend → Discord, redes, email, recordatorios |
| **Pagos (opcional/v2)** | MercadoPago / Stripe | Cuota de entrada por evento cuando aplique |
| **Infra** | Railway / Render (candidato) | Deploy de backend + Postgres + Redis administrados |

> **TypeScript de punta a punta** (app + admin + backend) es la gran ventaja: se comparten
> tipos y enums vía `packages/shared`, evitando desincronización entre cliente y servidor.

## Decisiones de arquitectura

1. **Monorepo** — todo en un repo, separado por carpetas. Ver [estructura](./06-estructura-monorepo.md).
2. **App = consumo / Admin = operación** — los resultados y la gestión NO se hacen desde
   la app móvil; se capturan en el Admin Web por roles staff/caster/admin.
3. **Motor de formatos en el backend** — Strategy por `formato` de evento. Ver
   [ADR-0001](./adr/0001-motor-de-formatos.md).
4. **Stack TypeScript end-to-end** — ver [ADR-0002](./adr/0002-stack-typescript.md).
5. **Auth con JWT.** Login social (Google/Apple/Xbox) + respaldo email/contraseña; el
   **teléfono** se verifica por **SMS OTP** en "Completa tu perfil". Ver
   [ADR-0003](./adr/0003-auth-social-y-perfil.md).
6. **Automatización desacoplada con n8n** — el backend emite webhooks/eventos; n8n hace el
   trabajo de integración (Discord, redes, email) sin acoplar el core.

## Estrategia de lanzamiento móvil

App nativa primero (React Native / Expo). Riesgo principal = revisión de App Store.
Mitigación: subir a revisión ~2 semanas antes del 6 oct y correr beta con la comunidad vía
**TestFlight (iOS)** + **Play Internal Testing (Android)**. Ver [roadmap](./05-roadmap.md).

## Estándares técnicos

- **API:** JSON, `camelCase`, prefijo `/api/v1`, errores `{ code, message, details }`.
- **IDs:** UUID · **Fechas:** ISO-8601 UTC.
- **Auth:** JWT (access + refresh); roles `PLAYER`, `STAFF`, `ADMIN`.
- **Validación:** DTOs con `class-validator` en NestJS.
