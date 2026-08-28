# 05 · Roadmap hacia el lanzamiento

## Meta

App de Temp League disponible para la comunidad **alrededor del 6 de octubre de 2026**
(lanzamiento de *Gears of War: E-Day*), corriendo la **Temporada 1** de punta a punta.

## Restricción de tiempo

- **Hoy:** 28 de agosto de 2026
- **Lanzamiento del juego:** 6 de octubre de 2026
- **Ventana de desarrollo:** ~5.5 semanas

Es apretado. La disciplina de alcance (ver [MVP](./01-vision-producto.md#alcance-del-mvp-temporada-1))
es lo que hace la diferencia entre llegar y no llegar.

## Plan semana a semana

### Semana 1 (28 ago – 3 sep) · Fundaciones
- [ ] Inicializar `apps/backend` (Spring Boot + Postgres, migraciones)
- [ ] Entidades + enums del [modelo de datos](./03-modelo-datos.md)
- [ ] Auth (registro, login, JWT, roles)
- [ ] CRUD de usuarios y equipos
- [ ] Definir design system / branding de la app (colores, tipografía, componentes)

### Semana 2 (4 – 10 sep) · Eventos e inscripciones
- [ ] CRUD de eventos (tipo, formato, estados)
- [ ] Flujo de inscripción de equipos + aprobación admin
- [ ] Inicializar `apps/mobile` (Flutter): login + navegación base
- [ ] Pantallas: registro, crear/unirse a equipo, listar eventos

### Semana 3 (11 – 17 sep) · Motor de formatos y partidos
- [ ] Motor de formatos: `SINGLE_ELIM` + `ROUND_ROBIN` (mínimo viable)
- [ ] Generación de bracket / calendario al iniciar evento
- [ ] Captura de resultados + propagación de ganador / tabla
- [ ] App: vista de evento (equipos, bracket/tabla, mis partidos)

### Semana 4 (18 – 24 sep) · Consola admin + cierre de features
- [ ] Inicializar `apps/admin-web` (React)
- [ ] Admin: crear evento, aprobar equipos, generar bracket, capturar resultados
- [ ] `DOUBLE_ELIM` / `LEAGUE_PLAYOFFS` si el tiempo lo permite (si no, v2)
- [ ] Integración end-to-end app ↔ backend ↔ admin

### Semana 5 (25 sep – 1 oct) · Beta con la comunidad
- [ ] Deploy de backend (Railway/Render) + Postgres
- [ ] **Subir app a revisión** (App Store + Play Store) — margen para rechazos de Apple
- [ ] Beta cerrada vía TestFlight (iOS) + Play Internal Testing (Android)
- [ ] Recoger feedback de la comunidad, corregir bugs críticos
- [ ] Pulido de UI/UX

### Semana 6 (2 – 6 oct) · Lanzamiento 🚀
- [ ] Correcciones finales del feedback de beta
- [ ] Publicar app en tiendas
- [ ] Abrir inscripciones de **Temporada 1**
- [ ] Anuncio a la comunidad alineado al lanzamiento de E-Day

## Gestión de riesgos

| Riesgo | Impacto | Mitigación |
|--------|---------|------------|
| Revisión de App Store tardía/rechazo | Alto | Subir 2 semanas antes; beta por TestFlight en paralelo |
| Sobre-construcción (scope creep) | Alto | Congelar el MVP; formatos extra y features ricas → v2 |
| Motor de formatos más complejo de lo previsto | Medio | Empezar con `SINGLE_ELIM` + `ROUND_ROBIN`; los demás después |
| Comunidad no adopta self-service | Medio | Onboarding simple + acompañamiento en la beta |
| 1 solo dev / tiempo limitado | Medio | Aprovechar IA para acelerar; priorizar backend→app→admin |

## Estrategia de corte (si el tiempo aprieta)

Orden de sacrificio, del primero al último en cortarse:
1. `DOUBLE_ELIM` y `LEAGUE_PLAYOFFS` (dejar solo single-elim + round-robin)
2. Consola admin web → reemplazar temporalmente por endpoints/Postman para operar
3. iOS → lanzar solo Android primero (aprobación más rápida)

> Nunca se corta: inscripción self-service + ver evento/partidos en la app. Ese es el core.

## Post-lanzamiento (v2 — Temporada 2+)

Estadísticas en vivo, chat, resúmenes de jornada con IA, integración de streaming, pagos
reales, landing pública.

**Tiers de equipos:** la infraestructura de **ranking** captura datos desde T1, pero los
**tiers** (Tier 1/2/3) solo son significativos con historial → se activan de forma real en
**T2** para emparejamientos balanceados. Ver
[14-ranking-salon-de-la-fama-y-palmares.md](./14-ranking-salon-de-la-fama-y-palmares.md).
