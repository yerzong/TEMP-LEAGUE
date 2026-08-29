# Temp League — Build con PlayPals UI Kit

Registro del rediseño de Temp League usando el kit comprado **PlayPals - Social Sports
Club Management UI Kit (UINeed)**, adaptado a los colores de Temp League.

## Archivos Figma
- **Kit / librería:** PlayPals (fileKey `q12wwwBBtwdYsDwUYURS6c`). Editable (cuenta de
  estudiante). Se republica tras cada ajuste al design system.
- **App / trabajo:** Temp League (fileKey `rZRkj7kujPIZGZBMR5hzFh`). Aquí se construyen las
  pantallas usando la librería.
- **Cuenta Figma:** Dilan Yovani Dev (l23090940@zacatepec.tecnm.mx), student, Full seat.

## Adaptación de colores (hecha en el kit)
- **Brand** verde → **rojo Gears** (`#FF2E3A` base, rampa de 11).
- **Neutros / fondos** (tenían tinte verde) → **gris acero frío** (Card `#25272b`,
  Screen `#1b1d21`).
- **Semantic/Success** verde → rojo (badges OPEN/WIN).
- Barrido de verde "hardcodeado" en toda la página Design → rojo (78 fills + 2 strokes).
- Logo → Temp League; nombre → TEMP LEAGUE. Fuente del kit: **Lexend**.

## Componentes del kit (keys para importar)
- Button (set): `3732cb23b5289836288c2faf4b5e4fa64753aacf` — Type: Primary/Secondary/Danger; Style/Size/State; props Text, iconos.
- **Text Field** (set): `867d651b79f3a81a228ea46f11fa475ac32ff639` — Type: Default/Password/Phone Number/Code; State: Empty/Active/Filled.
- IconButton (set): `5dd90b1d0a53650e5053f4dd8a011e4df392803f`
- Avatar (set): `3b779dfad6281283555ee10665434f8c0a7ca18d`
- Club Avatar (set): `247f16e6fcc5e982860a8a7fe0ad0569d1f178a1`
- Card Match: `70ba98ee89b8f5f06ec088af9b0425b62fb4f94e`
- Card Competition: `3eb97415e702a2feac62bf98d484dcb2ac6eaf2e`
- Card Player (set): `ccae0cda4f4d47b28a189b7e135dd74f2c43666e`
- Card Stat: `9310c291b03a671a3d4cf140bced64732716043d`
- Card Bracket (set): `336f21a9643a946aa04f784b73060f059406ab80`
- Card Notification (set): `b3b2d14c3e90b3adf051fcaca48480ddec9aefbb`
- Card Club (set): `e060ccc1e298143a19a404623d303053227a91bf`

## Regla de trabajo (Gerson)
- **Cada pantalla en su página** (una por flujo).
- **Componente que falte → crearlo EN PlayPals** (usando piezas existentes o pegándose al
  estilo), luego republicar y usarlo. No inventar estilos fuera del kit.
- Documentar todo aquí y en commits.

## Estructura de páginas (Temp League)
- App · Onboarding + Auth (Splash, Onboarding, Login, Registro, OTP)
- App · Home
- (se agregan más flujos: Competiciones, Equipo, Perfil…)

## Pantallas construidas
- **00 · Splash** — bg acero + logo Temp League + wordmark. (pág. Onboarding + Auth)

## Pendientes / notas
- La Home v1 (armada antes del recolor) quedó verde; requiere aceptar "Update available"
  de la librería en Temp League para tomar el rojo.
- Componentes faltantes detectados: (ninguno confirmado aún; el Text Field SÍ existe).
