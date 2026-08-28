# ADR-0001 · Motor de formatos de evento

- **Estado:** Aceptado
- **Fecha:** 2026-08-28
- **Contexto:** MVP de Temp League

## Contexto

Temp League debe soportar, desde el día 1, dos tipos de evento con reglas de competencia
distintas:

- **Temporada** — round-robin (todos contra todos) + playoffs. Larga.
- **Relámpago** — eliminación directa (single/double). Corta.

Y debe poder crecer a más formatos sin reescribir el sistema. Programar cada tipo de
torneo por separado (código dedicado por evento) generaría duplicación y frenaría la
adición de formatos futuros.

## Decisión

El backend implementa un **motor de formatos** basado en el patrón *Strategy*. El evento
guarda un campo `formato` (`SINGLE_ELIM | DOUBLE_ELIM | ROUND_ROBIN | LEAGUE_PLAYOFFS`).
Una interfaz común —conceptualmente `GeneradorDeFormato`— tiene una implementación por
formato, y cada una sabe:

1. **Generar la estructura** de partidos a partir de las inscripciones aprobadas
   (rondas/jornadas, seeds, byes, enlaces `siguientePartidoId`).
2. **Procesar un resultado** de partido (propagar ganador en eliminación; recalcular
   tabla en liga).
3. **Exponer la vista** de progreso (bracket o tabla de posiciones).

Al pasar un evento de `INSCRIPCIONES` a `EN_CURSO`, el backend selecciona la estrategia
según `formato` y genera los partidos.

## Consecuencias

**Positivas**
- Agregar un formato nuevo = una implementación nueva de la interfaz, sin tocar el resto.
- Un solo flujo de inscripción, partidos y resultados sirve para todos los formatos.
- La app y el admin no necesitan lógica por tipo de evento; solo renderizan bracket o tabla.

**Negativas / a vigilar**
- La interfaz común debe cubrir bien tanto eliminación como liga (dos modelos mentales
  distintos: bracket con avance vs. tabla acumulativa). Se mitiga manteniendo el contrato
  mínimo y específico.
- Casos borde (número de equipos no potencia de 2, empates en liga) deben resolverse por
  estrategia.

## Alternativas consideradas

- **Código dedicado por tipo de evento:** rechazado por duplicación y baja extensibilidad.
- **Motor externo / librería de brackets:** rechazado para el MVP por falta de control y
  por acoplar el dominio a una dependencia; se puede reconsiderar en v2.
