# Temp League 🎮

Plataforma de ligas y torneos competitivos para la comunidad de **Gears of War** en LATAM.

Temp League permite a jugadores y equipos inscribirse, competir y dar seguimiento a
temporadas y eventos relámpago desde una **app móvil**, mientras la liga se administra
por completo desde una **consola web de administración**. Todo el proceso —hoy manual
en la escena de Gears— queda automatizado.

> **Objetivo de lanzamiento:** coincidir con el lanzamiento de *Gears of War: E-Day*
> (6 de octubre de 2026).

---

## 🧭 Índice de documentación

Toda la planeación vive en [`/docs`](./docs). Léela en este orden:

| # | Documento | Qué contiene |
|---|-----------|--------------|
| 00 | [Glosario](./docs/00-glosario.md) | Términos y vocabulario del dominio |
| 01 | [Visión de producto](./docs/01-vision-producto.md) | Problema, diferenciadores, usuarios, alcance MVP |
| 02 | [Arquitectura y stack](./docs/02-arquitectura.md) | Stack, diagrama de sistema, decisiones técnicas |
| 03 | [Modelo de datos](./docs/03-modelo-datos.md) | Entidades, relaciones, formatos de evento, estados |
| 04 | [Especificación de API](./docs/04-api-spec.md) | Endpoints core del backend |
| 05 | [Roadmap](./docs/05-roadmap.md) | Plan semana a semana hacia octubre |
| 06 | [Estructura del monorepo](./docs/06-estructura-monorepo.md) | Qué vive en cada carpeta |
| 07 | [Spec App Jugadores](./docs/07-app-jugadores-spec.md) | Pantallas, campos, flujos de la app móvil |
| 08 | [Spec Admin Web](./docs/08-admin-web-spec.md) | Módulos y permisos de la consola admin/staff |
| 09 | [Reglas de negocio](./docs/09-reglas-negocio.md) | Roles, unicidad, 1-equipo-por-jugador, ventana 72h, flujos |
| 10 | [Notificaciones y automatización](./docs/10-notificaciones-y-automatizacion.md) | Push (FCM) y automatización con n8n |
| 11 | [Flujo de generación de eventos](./docs/11-flujo-generacion-eventos.md) | Motor de formatos: sorteo, bracket/tabla, calendario auto + ajuste |
| 12 | [Formato de partido y día de partido](./docs/12-formato-partido-y-dia-de-partido.md) | Roster configurable, best-of por mapa, check-in/walkover, invitaciones |

Las decisiones de arquitectura relevantes se registran como ADRs en [`/docs/adr`](./docs/adr):
- [ADR-0001](./docs/adr/0001-motor-de-formatos.md) — Motor de formatos de evento
- [ADR-0002](./docs/adr/0002-stack-typescript.md) — Stack TypeScript end-to-end (NestJS/RN/Prisma)
- [ADR-0003](./docs/adr/0003-auth-social-y-perfil.md) — Autenticación social + perfil verificado

---

## 🗂️ Estructura del repo

```
temp-league/
├── apps/
│   ├── backend/      # API REST — NestJS + Prisma + PostgreSQL
│   ├── mobile/       # App de jugadores — React Native (Expo)
│   └── admin-web/    # Consola admin/staff — React
├── packages/
│   └── shared/       # Tipos/contratos compartidos (DTOs, enums)
├── docs/             # Toda la planeación y documentación
└── assets/           # Logos, branding, mockups
```

## 🚀 Estado actual

**Fase:** Planeación y documentación (sin código de aplicación todavía).

Ver el [Roadmap](./docs/05-roadmap.md) para el plan de ejecución.

## 🛠️ Stack

TypeScript de punta a punta:

- **Backend:** NestJS (Node + TypeScript) · Prisma · PostgreSQL · Redis
- **App móvil:** React Native (Expo) — experiencia de consumo del jugador
- **Admin web:** React + TypeScript — operación de la liga (captura de resultados)
- **Push:** Firebase Cloud Messaging · **SMS OTP:** Twilio/MessageBird
- **Automatización:** n8n (Discord, redes, email)
- **Infra:** por definir (Railway / Render candidatos)

---

_Temp League — hecho para la comunidad de Gears en LATAM._
