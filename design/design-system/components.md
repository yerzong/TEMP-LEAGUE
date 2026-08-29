# Componentes — Temp League

Inventario de componentes a diseñar, con sus estados. Derivado de las specs de app/admin
(`/docs/07`, `/docs/08`). Cada componente usa **tokens**, nunca valores sueltos.

## Estado de construccion (Figma, pagina "Components")

Librería reconstruida sobre la direccion final (Liquid Glass + rojo). Todo bindeado a
tokens. Atomos y genericos primero; los moleculares componen los atomos como instancias.

Atomos:
- Button (Variant: Primary/Glass/Ghost) + label
- Chip (Tone: Brand/Success/Warning/Info/Neutral) + label
- Stat Tile (props value, label)
- Icon Button (glass, slot "icon")
- Back Button (glass + chevron)
- Avatar (circular, fill = imagen)
- Input (State: Default/Focus/Error) + text
- Logo Tile (tile claro, slot "image")
- Section Header (prop label)
- Divider
- Tab Bar Item (State: Active/Inactive) + label

Moleculares (genericos):
- Card (panel glass generico)
- List Row (leading + title/subtitle + trailing; props title/subtitle)
- Match Card (compone Chip + Logo Tile + Button)
- Tab Bar (flotante glass; compone 5 Tab Bar Item)

## Paginas del proyecto Figma (por categoria)
Cover · Foundations · Components · App — Jugadores · Admin — Web · Brand (logo).

## Pantallas construidas (App — Jugadores)
Flujo inicial (component-driven): 00 Splash · 00b Onboarding · 01 Bienvenida · 02 Login ·
03 Completa tu perfil · 04 Verificacion OTP. Ademas: Home (final, glass + contenido rico).

## Foundations
Pagina Foundations con swatches de color bindeados a tokens (+ hex), escala tipografica
(Inter) y espaciado 8pt.

## Pendiente
- Reconstruir Home con instancias de Match Card / Stat Tile (anti-drift total).
- Mas pantallas: Eventos, Detalle de partido, Mi Equipo, Perfil.
- Assets de imagen (logos/fotos/iconos) viven en Figma; los exports/screenshots en
  design/references/exports/.

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
