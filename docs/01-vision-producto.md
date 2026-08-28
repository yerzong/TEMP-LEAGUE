# 01 · Visión de producto

## El problema

La escena competitiva de **Gears of War** en LATAM se organiza de forma **100% manual**:
inscripciones por Discord/formularios, brackets en Excel o Challonge, resultados
capturados a mano, comunicación dispersa. No existe una plataforma que unifique el
proceso. La única competencia directa (la "LPL de Gears") también opera manualmente.

Con el lanzamiento de **Gears of War: E-Day (6 oct 2026)**, la comunidad se reactivará.
Es la ventana perfecta para llegar con algo mejor.

## La oportunidad

Ser **la primera plataforma automatizada** de ligas de Gears en LATAM. El diferenciador
no es "hacer torneos" (eso ya existe), es **eliminar la fricción**: que inscribirse,
competir y seguir la liga se sienta como un producto profesional, no como un torneo
improvisado.

## Diferenciadores clave

1. **Automatización real** — generación de brackets/tablas, calendario y captura de
   resultados sin Excel.
2. **Un motor, múltiples formatos** — Temporadas largas *y* eventos Relámpago desde el
   mismo sistema. Ver [modelo de datos](./03-modelo-datos.md).
3. **Experiencia móvil pulida** — el jugador vive la liga desde su celular.
4. **Marca profesional** — diseño cuidado (ventaja de tener UI/UX in-house).
5. **Potenciado con IA** — scheduling automático, resúmenes de jornada para redes,
   onboarding conversacional. Ver [ideas de IA](#dónde-entra-la-ia).

## Usuarios y roles

| Rol | Qué hace | Dónde |
|-----|----------|-------|
| **Jugador** | Se registra, se une a un equipo, ve sus partidos, resultados y la liga | App móvil |
| **Capitán** | Todo lo del jugador + crea/gestiona su equipo, lo inscribe a eventos | App móvil |
| **Admin** | Crea eventos, aprueba equipos, genera brackets, captura resultados, gestiona la liga | Consola web |

## Tipos de evento (corazón del producto)

La plataforma no maneja "un formato": maneja **eventos con formatos distintos**.

| Tipo | Duración | Formato típico | Ejemplo |
|------|----------|----------------|---------|
| **Temporada** | Semanas | Round-robin + Playoffs | "Temp League Temporada 1" |
| **Relámpago** ⚡ | 1 día / fin de semana | Eliminación directa (single/double) | "Copa E-Day Launch" |

Esta flexibilidad es lo que nos diferencia y lo que hace escalable el producto: agregar
un formato nuevo no requiere reescribir el sistema.

## Alcance del MVP (Temporada 1)

### ✅ Dentro del MVP
- Autenticación (jugadores + admins)
- Registro de jugadores y creación/gestión de equipos
- Inscripción de equipos a un evento (con aprobación del admin)
- Vista de evento: equipos inscritos + rosters
- Generación de **bracket** (eliminación) y **tabla** (liga)
- Calendario de partidos y **captura de resultados** (admin captura → jugador consulta)
- Consola admin para operar todo lo anterior

### ❌ Fuera del MVP (Temporada 2+)
- Estadísticas en vivo / integración con APIs de Gears
- Ranking ELO / MMR
- Notificaciones push
- Chat interno
- Perfiles enriquecidos (historial, logros)
- Pagos / inscripciones de paga
- Streaming / integración con Twitch

> Regla de oro del MVP: **lanzar funcional a tiempo > lanzar completo tarde.**

## Dónde entra la IA

No como gimmick, como valor real:
- **Scheduling automático** de partidos evitando choques de horario.
- **Resúmenes de jornada** generados para publicar en redes (motor de contenido = crecimiento).
- **Onboarding conversacional** para inscribir equipos.
- **Seeding inteligente** basado en historial.
- **Aceleración del desarrollo** (asistencia de IA en todo el build).

## Métrica de éxito del lanzamiento

- Temporada 1 corriendo con **≥ 8 equipos** inscritos vía la plataforma.
- Inscripción y consulta 100% self-service (cero Excel).
- App disponible en tiendas para el 6 de octubre.
