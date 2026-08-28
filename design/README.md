# 🎨 Diseño — Temp League

Espacio de trabajo de UI/UX. El **cerebro** ([`00-design-brain.md`](./00-design-brain.md))
es la fuente única de verdad; todo lo demás cuelga de ahí.

## Estructura

```
design/
├── 00-design-brain.md      # 🧠 CEREBRO: principios, dirección, cómo trabajan los agentes
├── brand/                  # Identidad: logo, paleta oficial, tipografías (a completar)
├── design-system/          # tokens.md + components.md (design tokens y librería)
├── flows/                  # Flujos UX derivados de /docs
├── references/             # Moodboard, inspiración, notas
└── figma/                  # Enlace y exports del proyecto Figma (PENDIENTE — no tocar aún)
```

## Agentes de diseño (`.claude/agents/`)

- **ux-flow-designer** — flujos e IA, wireframes low-fi
- **ui-visual-designer** — alta fidelidad, marca y tokens
- **design-system-architect** — tokens y componentes

## Skill

- **design-review** (`.claude/skills/`) — revisa una propuesta de UI contra el cerebro y el
  design system.

## Regla de oro

1. Lee el cerebro + `/docs` antes de diseñar.
2. Usa tokens, no valores sueltos.
3. **No tocar Figma** hasta que el proyecto esté disponible.
