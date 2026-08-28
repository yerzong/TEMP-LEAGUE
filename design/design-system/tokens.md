# Design Tokens — Temp League

Valores base compartidos por app y admin. Se **confirman con Figma**; aquí la estructura y
una hipótesis inicial. Nada de valores sueltos en el diseño: todo sale de aquí.

## Color (hipótesis — confirmar con Figma)
| Token | Uso | Valor tentativo |
|-------|-----|-----------------|
| `color.bg` | Fondo base (oscuro) | carbón `#0E0F12` |
| `color.surface` | Superficies/cards | acero `#1A1D22` |
| `color.primary` | Acción principal / marca | rojo/naranja E-Day `#FF4D1C` |
| `color.text` | Texto principal | `#F2F3F5` |
| `color.textMuted` | Texto secundario | `#9AA0A6` |
| `color.success` | Estado ok | `#31C48D` |
| `color.warning` | Aviso | `#F5A524` |
| `color.danger` | Error/derrota | `#F04438` |
| `color.border` | Bordes/divisores | `#2A2E35` |

## Tipografía
| Token | Uso |
|-------|-----|
| `font.display` | Títulos con carácter |
| `font.body` | Texto y datos (legible) |
| Escala | `xs, sm, md, lg, xl, 2xl, 3xl` (definir px/line-height con Figma) |

## Espaciado
Escala base 4px: `space.1=4 · space.2=8 · space.3=12 · space.4=16 · space.6=24 · space.8=32`.

## Radios / elevación
- `radius.sm/md/lg`, `radius.full` (avatares/chips).
- Sombras sutiles (tema oscuro depende más de superficie/borde que de sombra).

## Estados semánticos (chips)
Inscripción: pendiente/aprobada/rechazada · Partido: programado/en vivo/finalizado ·
Tier: 1/2/3 · Bloqueos: perfil incompleto, ventana 72h, sanción activa.

> Estos tokens se materializarán como variables en el código (RN/React) y, cuando llegue,
> se sincronizan con las **variables de Figma**.
