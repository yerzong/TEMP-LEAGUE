# Design Tokens — Temp League

Fuente de verdad de los tokens. **Implementados como variables/estilos en Figma** (archivo
"Prueba IA") y a materializar igual en código (RN/React). Dirección: **calidad Apple + piel
Gears**. Nada de valores sueltos: todo sale de aquí.

## Color — colección `Color` (modo **Dark**)

| Token (Figma) | Hex | Uso | Scopes |
|---------------|-----|-----|--------|
| `surface/bg` | `#0E0F12` | Fondo base (carbón) | FRAME/SHAPE fill |
| `surface/surface` | `#16181D` | Cards / superficies | FRAME/SHAPE fill |
| `surface/elevated` | `#1E2127` | Superficie elevada | FRAME/SHAPE fill |
| `border/default` | `#2A2E35` | Bordes / divisores | STROKE |
| `text/primary` | `#F2F3F5` | Texto principal | TEXT fill |
| `text/muted` | `#9AA0A6` | Texto secundario | TEXT fill |
| `text/on-primary` | `#FFFFFF` | Texto sobre acento | TEXT fill |
| `brand/primary` | `#FF5A1F` | Acción principal / marca (E-Day) | fill/text/stroke |
| `brand/primary-pressed` | `#E64A12` | Estado presionado | FRAME/SHAPE fill |
| `status/success` | `#2ECC8F` | Éxito / victoria | fill/text |
| `status/warning` | `#F5A524` | Aviso | fill/text |
| `status/danger` | `#E5484D` | Error / derrota | fill/text |
| `status/info` | `#3B9EFF` | Info | fill/text |
| `*-tint` (6) | pre-mezcla 16% sobre `bg` | Fondos tintados de chips (success/warning/danger/info/brand/muted) | FRAME/SHAPE fill |

> **Tint tokens:** `status/success-tint #132D26 · warning-tint #332715 · danger-tint #30181B ·
> info-tint #152638 · brand/primary-tint #351B14 · text/muted-tint #26262A`. Son sólidos
> (el 16% ya viene "horneado") porque la opacidad de un paint **no se hereda bien en
> instancias** de Figma — con tokens sólidos, las instancias siempre se ven correctas.

> Modo único **Dark** por ahora. La estructura permite añadir un modo claro después sin
> tocar los componentes (solo se agregan valores al mismo token).

## Números — colección `Number`

**Espaciado (grid 4/8pt)** — scopes GAP + WIDTH_HEIGHT:
`space/1=4 · space/2=8 · space/3=12 · space/4=16 · space/5=20 · space/6=24 · space/8=32 · space/10=40`

**Radios** — scope CORNER_RADIUS:
`radius/sm=8 · radius/md=12 · radius/lg=16 · radius/full=999`

## Tipografía — estilos de texto

**Display/Títulos → Saira Condensed** · **Texto → Inter** (calidad Apple, base 17px).

| Estilo (Figma) | Fuente | Tamaño / interlínea |
|----------------|--------|---------------------|
| `Display` | Saira Condensed Bold | 44 / 48 |
| `Heading/H1` | Saira Condensed Bold | 34 / 40 |
| `Heading/H2` | Saira Condensed SemiBold | 28 / 34 |
| `Heading/Title` | Saira Condensed SemiBold | 22 / 28 |
| `Text/Body` | Inter Regular | 17 / 24 |
| `Text/Callout` | Inter Regular | 16 / 22 |
| `Text/Subhead` | Inter Medium | 15 / 20 |
| `Text/Footnote` | Inter Regular | 13 / 18 |
| `Text/Caption` | Inter Regular | 12 / 16 |

## Elevación / sombra
Tema oscuro: se apoya más en **superficie + borde** que en sombra. Sombras sutiles solo
para overlays/menus (definir al crear esos componentes).

## Estados semánticos (chips)
Inscripción: pendiente/aprobada/rechazada · Partido: programado/en vivo/finalizado ·
Tier: 1/2/3 · Bloqueos: perfil incompleto, ventana 72h, sanción activa.

> Sincronización código↔Figma vía `/sync-figma-token` cuando exista el tema en código.
