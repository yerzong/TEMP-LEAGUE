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

## Skills (`.claude/skills/`)

Propias:
- **design-review** — revisa una propuesta de UI contra el cerebro y el design system.

Instaladas (anti-drift, de terceros — ver créditos):
- **audit-design-system** — audita un nodo de Figma buscando deriva (componentes no
  reutilizados, overrides locales, tokens sin enlazar). Solo lectura.
- **apply-design-system** — reconecta elementos sueltos a los componentes/tokens del sistema.
- **fix-design-system-finding** — corrige un hallazgo específico de la auditoría.
- **sync-figma-token** — sincroniza tokens entre código y variables de Figma (con reporte de
  drift y gate de aprobación). Se invoca manualmente: `/sync-figma-token`.

### Créditos y fuentes
- `audit/apply/fix-design-system-*` — Chris Goebel · Edenspiekermann
  ([github.com/edenspiekermann/Skills](https://github.com/edenspiekermann/Skills))
- `sync-figma-token` — Anish Karthik · Firebender
  ([github.com/firebenders/sync-figma-token-skill](https://github.com/firebenders/sync-figma-token-skill))
- Colección: [Figma — 10 Claude Skills for Design](https://www.figma.com/resource-library/claude-skills-for-design/)

> Estas skills usan las herramientas MCP de Figma; **operan sobre Figma solo cuando se
> invocan explícitamente** y el proyecto esté disponible.

## Regla de oro

1. Lee el cerebro + `/docs` antes de diseñar.
2. Usa tokens, no valores sueltos.
3. **No tocar Figma** hasta que el proyecto esté disponible.
