# 🧠 Design Brain — Temp League

**Fuente única de verdad del diseño.** Cualquier persona o agente de IA que diseñe para
Temp League lee esto **primero**. Mantiene la coherencia entre la app, el admin y la marca.

> Este cerebro NO reemplaza a Figma: cuando exista el proyecto de Figma, **Figma es la
> verdad visual** y este documento la contextualiza y la conecta con el producto.

---

## 1. Qué estamos diseñando

- **App de Jugadores** (React Native · móvil): experiencia de **consumo** — inscribirse,
  seguir eventos, ver partidos/bracket/tabla, gestionar equipo. Mobile-first, emocional.
- **Consola Admin/Staff** (React · web): herramienta de **operación** — data-dense,
  eficiente, tablas y formularios claros. Prioridad: capturar un resultado en 2-3 clics.

Contexto de producto completo en [`/docs`](../docs) (léelo, no inventes features):
producto, modelo de datos, reglas, flujos, formato de partido, ranking, etc.

## 2. Principios de diseño

1. **Competitivo y serio, no infantil.** Estética de esports profesional (piensa LCK/LLA),
   con la identidad cruda de Gears/E-Day.
2. **Claridad sobre decoración.** El jugador debe ver *su próximo partido* y *su equipo* al
   instante. El admin debe operar sin fricción.
3. **Consistencia por tokens.** Nada de valores sueltos: color, tipografía y espaciado
   salen del design system.
4. **Mobile-first en la app; densidad controlada en el admin.**
5. **Estados siempre visibles:** vacío, cargando, error, bloqueado (ej. ventana de
   transferencia, perfil incompleto, sanción activa).
6. **Accesibilidad:** contraste suficiente en tema oscuro, áreas táctiles ≥ 44px, texto
   legible.

## 3. Dirección visual — "Revolut premium, rojo Temp League" ⭐

**Principio rector:** estructura/comportamiento con **calidad Apple**, look **premium tipo
Revolut** (dark, espacioso, esquinas grandes, sin bordes duros), y **rojo** como color de
marca. App que se siente cara y confiable, inconfundiblemente Temp League.

> ⚠️ Actualizado tras feedback: se descartó el naranja + Saira Condensed. Todo en **una
> sola página** de Figma. Imágenes reales (no placeholders).

**De Apple (HIG) tomamos** — el "cómo":
- Estructura, jerarquía y patrones de navegación probados.
- **Grid y espaciado 8pt**, tipografía impecable y legible.
- Componentes con **comportamiento nativo**, microinteracciones y motion sobrios.
- **Accesibilidad** (contraste, áreas táctiles ≥44px, dynamic type) y claridad.

**De Gears/E-Day tomamos la piel** — el "cómo se ve":
- Tema **oscuro dominante** (acero/carbón) + acento **rojo/naranja E-Day**.
- Textura industrial/metálica, tono competitivo, bordes marcados, contraste alto.

**Lo que NO hacemos:**
- No copiamos el look **claro/translúcido/calmado** de Apple que borraría la identidad Gears.
- **Liquid Glass** se usa con mesura y **teñido a la marca** (no como estética dominante).

- **Referencia de estilo (confirmada):** **Revolut** — dark premium, mucho aire, esquinas
  grandes (20–24), sin bordes duros (superficies en lugar de líneas), jerarquía fuerte,
  números grandes. Calidad Apple en estructura/comportamiento.
- **Tipografía (confirmada):** **Inter** en todo (limpia, premium, pesos fuertes para
  display/números). *(Saira Condensed se descartó: muy deportiva para el look Revolut.)*
- **Acento (confirmado):** **ROJO `#FF2E3A`** (color principal de Temp League) sobre base
  casi negra `#0A0B0D`.
- **Imágenes:** avatares/logos reales (se generan vía API: DiceBear) e íconos reales
  (lucide vía Iconify) — nada de placeholders.

> Regla mental: **estructura/comportamiento = Apple · vestido/color/tono = Gears.**

## 4. Marca

Identidad (logo, paleta oficial, tipografías) → [`brand/`](./brand/). A completar con los
assets reales de Temp League y el proyecto de Figma.

## 5. Design system

Tokens y componentes → [`design-system/`](./design-system/). Los componentes clave a
diseñar: botón, input, **card de partido**, **bracket**, **tabla de posiciones**, chips de
estado (inscripción, partido, tier), avatar/logo de equipo, badges de rol.

## 6. Flujos UX

Derivados de los docs de producto → [`flows/`](./flows/). Prioritarios:
onboarding (login social + completa tu perfil), crear org/equipo + inscribir, día de
partido (check-in), seguir evento (bracket/tabla).

## 7. Cómo trabajan los agentes de diseño

Tres agentes especializados (en `.claude/agents/`):
- **`ux-flow-designer`** — flujos, arquitectura de información, wireframes de baja fidelidad.
- **`ui-visual-designer`** — dirección visual, alta fidelidad, aplicar marca y tokens.
- **`design-system-architect`** — tokens y librería de componentes, coherencia.

Reglas para todos:
1. **Lee `/docs` y este cerebro antes de diseñar.** No inventes features fuera del alcance.
2. **Usa tokens, no valores mágicos.**
3. **Respeta los estados y las reglas de negocio** (gating, bloqueos, roles).
4. **No toques Figma** hasta que el proyecto esté disponible y se indique explícitamente.
5. Cuando exista Figma, **aliníate a Figma** como verdad visual.
6. **Estructura/comportamiento = Apple (HIG) · piel = Gears.** Aplica las skills de Apple
   (`apple-hig-designer`, `apple-design`) para calidad, patrones y accesibilidad — pero el
   color, tono y textura salen de la piel Gears vía tokens. Nunca dejes que el look "Apple
   limpio" sobrescriba la identidad de marca.

Skills anti-drift y de estilo disponibles en `.claude/skills/` (ver `design/README.md`):
consistencia (`audit`/`apply`/`fix-design-system`, `sync-figma-token`), estilo Apple
(`apple-hig-designer`, `apple-design`) y revisión propia (`design-review`).

## 8. Estado actual

- ✅ Producto documentado (`/docs`).
- 🟡 Dirección visual: **hipótesis** (esperando Figma de Gerson).
- ⏳ Figma: **pendiente** — no intervenir todavía.
