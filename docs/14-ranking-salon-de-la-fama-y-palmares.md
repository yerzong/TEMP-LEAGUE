# 14 · Ranking, tiers, salón de la fama y palmarés

Progresión competitiva de la liga: reconocer a los campeones, construir el historial de
equipos/jugadores y nivelar los enfrentamientos con el tiempo.

## Campeón por evento

- Al pasar un evento a `FINALIZADO`, se fija su **campeón** (`Evento.campeonEquipoId`):
  - **Eliminación:** ganador de la final.
  - **Liga:** ganador de los playoffs (o 1º de la tabla si no hay playoffs).
- Este dato alimenta el Salón de la Fama y el palmarés.

## 🏆 Salón de la Fama

- Módulo que lista los **campeones de cada evento** (temporadas y relámpagos): equipo,
  roster campeón, evento y fecha.
- Se **llena solo** conforme terminan eventos (vacío al lanzar; crece con la liga).
- Ubicación en la app: dentro de **Eventos** (o acceso desde Inicio), no como tab nuevo.
- Es un **derivado** de eventos `FINALIZADO` + su `campeonEquipoId` (sin datos extra).

## 🎖️ Palmarés (historial en perfil)

- En el **perfil de equipo/organización** y de **jugador**: trofeos y participaciones
  (qué eventos ganó, en cuáles participó, resultados).
- Derivado de inscripciones + resultados + campeón por evento.
- Los **relámpagos cuentan** para el palmarés (un trofeo más), aunque no aporten ranking
  continuo.

## 👥 Perfil público de equipo

- Página del **equipo/organización** visible para todos: roster, palmarés, redes sociales,
  próximos partidos y ranking/tier actual.
- Sirve de "carta de presentación" de la marca en la comunidad.

## 📊 Ranking y Tiers

### Alcance por temporada
- **En el lanzamiento (T1):** la infraestructura de ranking **captura datos desde el primer
  partido**, pero los **tiers arrancan sin valor** (no hay historial). Se vuelven
  significativos conforme avanza T1 y, sobre todo, en **T2**.
- **T2 en adelante:** los tiers permiten **emparejamientos/seeding más nivelados** usando lo
  ocurrido en temporadas previas.

### Dos capas de clasificación
1. **Tabla de posiciones (in-season):** ya existe por evento de liga (round-robin). Mide el
   desempeño **dentro** de un evento.
2. **Ranking global (cross-evento):** acumula desempeño **entre** eventos para producir un
   ranking y asignar tiers.

### Cómo se calcula el ranking global (`RankingEquipo`)
Métricas acumuladas por equipo a lo largo de eventos finalizados:
- Partidos jugados, victorias, derrotas.
- **Diferencia de mapas** (mapas ganados − perdidos).
- **Puntos** (esquema configurable; ej. +3 por victoria de serie).
- **% de victorias**.
- **Posición final** en cada evento (peso extra a campeonatos/podios).

### Asignación de tiers
- Al **iniciar una nueva temporada**, se calculan los tiers a partir del ranking global
  acumulado (percentiles o umbrales):
  - `TIER_1` (fuerza alta) · `TIER_2` (media) · `TIER_3` (en desarrollo).
- Los tiers alimentan **seeds** (para separar favoritos) y **emparejamientos balanceados**.
- Un equipo **sin historial** entra como `TIER_3` / sin clasificar hasta acumular partidos.

> Nota: el ranking global se **recalcula al finalizar cada evento** (job en backend). Puede
> materializarse en `RankingEquipo` para rendimiento.

## ⚡ Relámpagos y ranking
- No generan ranking **continuo**; solo definen un **campeón**.
- Sí **cuentan para el palmarés** y **pueden aportar puntos** al ranking global (opcional,
  configurable), pero no son una clasificación en sí mismos.

## Invariantes (para tests)
- [ ] Al finalizar un evento se fija `campeonEquipoId` exactamente una vez.
- [ ] Salón de la Fama y palmarés se derivan solo de eventos `FINALIZADO`.
- [ ] El ranking global se recalcula al finalizar cada evento.
- [ ] Equipos sin historial no rompen el cálculo de tiers (entran sin clasificar/`TIER_3`).
