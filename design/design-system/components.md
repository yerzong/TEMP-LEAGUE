# Componentes — Temp League

Inventario de componentes a diseñar, con sus estados. Derivado de las specs de app/admin
(`/docs/07`, `/docs/08`). Cada componente usa **tokens**, nunca valores sueltos.

## Estado de construcción (Figma "Prueba IA")
- ✅ **Button** — set de 8 variantes (`Style`: Primary/Secondary/Ghost/Danger × `State`:
  Default/Disabled) + propiedad de texto `label`. Todo bindeado a tokens. Página
  "Components · Button". *(Pressed = opacidad en runtime RN, no variante.)*
- ⏳ Input · Chip/Badge · Card de partido · Bracket · Tabla — pendientes.

## Base
- **Botón** — ✅ construido (ver arriba). Variantes Style × State, label editable.
- **Input / Select** — con validación en vivo (ej. unicidad de nombre de org), error, ayuda.
- **Chip / Badge de estado** — inscripción, partido, tier, rol de equipo, bloqueos.
- **Avatar / Logo de equipo** — con fallback.
- **Card** — superficie base reutilizable.

## De dominio (los que dan identidad)
- **Card de partido** — equipos, marcador de serie, fecha, canal, botón de check-in / "ver
  en vivo". Estados: programado, en vivo, finalizado, walkover.
- **Bracket** — llaves de eliminación con avance del ganador; responsivo en móvil.
- **Tabla de posiciones** — liga: posición, PJ, V, D, dif. mapas, puntos.
- **Card de evento** — tipo (temporada/relámpago), estado, cupo, gratis/cuota, premio.
- **Roster** — lista de miembros con rol (capitán/jugador/suplente/coach).
- **Fila de captura de resultado (admin)** — captura por mapa (mapa+modo+ganador), rápida.

## Patrones
- **Estados vacío/carga/error** para cada lista.
- **Bloqueos con mensaje claro** (perfil incompleto, ventana 72h, sanción).
- **Tab bar** (app, 5 tabs) y **nav lateral** (admin, módulos).

> Al integrar Figma, cada componente se mapea a su componente de Figma (y, más adelante,
> a Code Connect en el código).
