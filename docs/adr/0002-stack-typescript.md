# ADR-0002 · Stack TypeScript end-to-end

- **Estado:** Aceptado
- **Fecha:** 2026-08-28
- **Reemplaza:** decisión inicial de Spring Boot + Flutter (documentada en versiones previas de arquitectura)

## Contexto

La primera propuesta usaba Spring Boot (Java) + Flutter (Dart) + React. Tras revisar,
el equipo (1 dev full-stack) prioriza **mantenibilidad, escalabilidad y velocidad** con un
solo lenguaje. Además se suman requisitos de automatización (n8n) y notificaciones push.

## Decisión

Adoptar **TypeScript de punta a punta**:

- **Backend:** NestJS (arquitectura modular, DI, escalable).
- **ORM/DB:** Prisma + PostgreSQL.
- **App móvil:** React Native (Expo).
- **Admin web:** React.
- **Contratos compartidos:** `packages/shared` (enums, DTOs, tipos) reutilizados por los tres.

Servicios de apoyo: Redis (OTP/colas), FCM (push), proveedor SMS (OTP), n8n (automatización).

## Consecuencias

**Positivas**
- Un solo lenguaje → menor carga cognitiva, tipos compartidos, refactors cruzados fáciles.
- NestJS impone estructura modular → mantenible al crecer.
- Prisma da migraciones y tipos generados desde el esquema.
- Ecosistema JS/TS enorme para push, pagos, SMS, etc.

**Negativas / a vigilar**
- Node es single-thread; para cargas pesadas (generación de brackets grandes) usar colas
  (Redis/BullMQ) si hace falta.
- Menos "opinado" que Spring en algunas cosas; se compensa con la estructura de NestJS.

## Alternativas consideradas

- **Spring Boot + Flutter:** robusto pero dos lenguajes (Java + Dart) para 1 dev; más
  fricción de contexto. Rechazado.
- **Backend Go/Python:** descartado por no unificar lenguaje con los clientes.
