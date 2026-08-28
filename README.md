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

Las decisiones de arquitectura relevantes se registran como ADRs en [`/docs/adr`](./docs/adr).

---

## 🗂️ Estructura del repo

```
temp-league/
├── apps/
│   ├── backend/      # API REST — Spring Boot + PostgreSQL
│   ├── mobile/       # App de jugadores — Flutter (iOS + Android)
│   └── admin-web/    # Consola de administración — React
├── packages/
│   └── shared/       # Tipos/contratos compartidos (DTOs, enums)
├── docs/             # Toda la planeación y documentación
└── assets/           # Logos, branding, mockups
```

## 🚀 Estado actual

**Fase:** Planeación y documentación (sin código de aplicación todavía).

Ver el [Roadmap](./docs/05-roadmap.md) para el plan de ejecución.

## 🛠️ Stack

- **Backend:** Java 21 · Spring Boot · PostgreSQL
- **App móvil:** Flutter (Dart)
- **Admin web:** React + TypeScript
- **Infra:** por definir (Railway / Render candidatos)

---

_Temp League — hecho para la comunidad de Gears en LATAM._
