# 06 · Estructura del monorepo

Todo vive en un solo repositorio, separado por carpetas. Cada `app` es desplegable de
forma independiente pero comparte contratos vía `packages/shared`.

```
temp-league/
├── README.md                 # Punto de entrada + índice de docs
├── .gitignore                # Ignora artefactos de Java, Node y Flutter
│
├── apps/
│   ├── backend/              # API REST — Spring Boot + PostgreSQL
│   │   └── (Java 21, Maven/Gradle, controllers, services, motor de formatos)
│   │
│   ├── mobile/               # App de jugadores — Flutter
│   │   └── (Dart, pantallas: login, equipos, eventos, partidos)
│   │
│   └── admin-web/            # Consola de administración — React + TS
│       └── (gestión de eventos, aprobación de equipos, captura de resultados)
│
├── packages/
│   └── shared/               # Contratos compartidos
│       └── (enums de formato/estado, definición de DTOs, colección de API)
│
├── docs/                     # Toda la planeación (este directorio)
│   ├── 00-glosario.md
│   ├── 01-vision-producto.md
│   ├── 02-arquitectura.md
│   ├── 03-modelo-datos.md
│   ├── 04-api-spec.md
│   ├── 05-roadmap.md
│   ├── 06-estructura-monorepo.md
│   └── adr/                  # Architecture Decision Records
│       └── 0001-motor-de-formatos.md
│
└── assets/                   # Logos, branding, mockups de la marca
```

## Convenciones

- **Cada `app/` tiene su propio README** con instrucciones de setup y ejecución local
  (se agregan cuando se inicializa el proyecto correspondiente).
- **`docs/` es la fuente de verdad de producto y arquitectura.** El código sigue a los
  docs, no al revés.
- **Decisiones importantes → ADR** en `docs/adr/`, numerados secuencialmente.
- **Nada de secretos en el repo.** Variables sensibles vía `.env` (ignorado); se versiona
  solo `.env.example`.

## Por qué monorepo (y no repos separados)

- Contratos backend↔cliente sincronizados en un solo lugar (`packages/shared`).
- Un solo historial, una sola fuente de verdad, contexto completo para el equipo (y la IA).
- Facilita cambios que cruzan capas (ej: agregar un campo que toca API, app y admin).
- El tamaño del proyecto no justifica la sobrecarga de múltiples repos.
