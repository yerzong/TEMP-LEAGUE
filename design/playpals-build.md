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
- App · Onboarding + Auth (Splash, Onboarding, Login, Registro, OTP, Completa perfil)
- App · Home (10 · Home)
- (se agregan más flujos: Eventos, Detalle de partido, Equipo, Perfil…)

## Componentes de navegación del kit (keys)
- Top Nav (component): `93c5c4f1fdd189653858bdbe32b0c7887ef3fe94`
- BottomBar (component): `62a2b4dd0850c24ec65497b4130c2230bfe8cbf5`
- Status Bar (set): `a0c410224e38bd59a8c0492bedf09ef309a132d3`
- Dropdown (set): en kit pág. Design System — usar para selects (Región, etc.)
- Tab Group Pills (component): `f0a627b4c7497c30a339955bfa7b6ae54fd5f618`

## Pantallas construidas
Página **App · Onboarding + Auth** (node 7:222). Todas 390x844, bg acero, Lexend, componentes del kit:
- **00 · Splash** — bg acero + LogoMark (ícono rojo) + wordmark TEMP LEAGUE.
- **01 · Onboarding** — panel rojo (gradiente) con LogoMark, título, subtítulo, page-dots, Button "Crear cuenta", link "Ya tengo cuenta".
- **02 · Login** — LogoMark, título, Text Field correo + Text Field password, link "Olvidaste tu contraseña", Button "Iniciar sesion", link "Crear cuenta".
- **03 · Registro** — IconButton back, 4 Text Field (nombre, correo, teléfono con bandera, password), nota de términos, Button "Crear cuenta".
- **04 · Verificacion OTP** — IconButton back, 4 Text Field Type=Code (SMS), reenvío con contador, Button "Verificar".
- **05 · Completa tu perfil** — Avatar con badge de edición, 3 Text Field (nombre, gamertag Xbox, región), Button "Entrar a Temp League".

Página **App · Home** (node 16:222):
- **10 · Home** — Top Nav ("Hola, Dilan" + bell), Card Match en vivo (NOVA ESPORTS vs TEAM AZTECA, 1-0, Mapa 2, EN VIVO), fila de 2 Card Stat (Temporada Tier 2 / 1240 pts, Historial winrate 68% / 25 partidos), Card Competition (Copa E-Day 2026, OPEN), BottomBar (Inicio/Eventos/Mi Equipo/Partidos/Perfil). Pendiente: reemplazar logos/foto de muestra del kit por imágenes reales de equipos/Gears.

Página **App · Eventos** (node 17:222):
- **20 · Eventos** — Top Nav "Eventos", filtros Tab Group Pills (Todos/Temporada/Relampago), lista de 4 Card Competition (Copa E-Day 2026, Temporada Clausura, Relampago Nocturno, Winter Warm-Up) con badges OPEN/LLENO/CERRADO, BottomBar.

Página **App · Detalle de partido** (node 18:222):
- **21 · Detalle de partido** — Top Nav "Partido en vivo", Card Match hero (NOVA 2-1 AZTECA, BO5 Mapa 4, EN VIVO), lista de 5 filas de resultado por mapa (Mapa, modo, ganador, score), Button "Ver transmision". Las filas de mapa son composición local (Card bg + texto) -> formalizar como componente PlayPals "Card Map Result".

Página **App · Mi Equipo** (node 19:222):
- **30 · Mi Equipo** — Top Nav "Mi Equipo", header de equipo (Club Avatar, NOVA ESPORTS, [NOVA] Tier 2, 12V-3D / 1240 pts), lista Roster con 6 Card Player Type=Default (Capitan/Jugador/Suplente), Button Secondary "Gestionar roster", BottomBar.

Página **App · Perfil** (node 20:222):
- **40 · Perfil** — Top Nav "Perfil", header (Avatar, Dilan Garcia, @DilanYG, chip TIER 2 · Capitan), 2 Card Stat (Partidos 87/68%, Ranking 1240/#42), menú de ajustes (6 filas: Mi cuenta, Notificaciones, Historial, Palmares, Ajustes, Cerrar sesion), BottomBar. Las filas de menú son composición local -> formalizar como "List Row" en PlayPals.

## Componentes NUEVOS creados en PlayPals (pág. Design System)
Creados pegados al estilo del kit (outline h58/radius24, Card Background, Neutral/*, Brand/50)
y bindeados a los paint styles del kit. **Pendiente: Gerson valida + republica la librería;
luego se insertan en las pantallas de Temp League por key.**
- ~~Social Button (mi versión con inicial)~~ **DESCARTADO por Gerson.** En su lugar se usa el
  patrón **SSO Auth** nativo del kit (node 391:13944), que ya trae logos reales:
  - Base: **Button** variante `Type=Secondary, Style=Default` (key `e41b683a17a016fb9dbd8ad8322512e2bf3e5e2f`; el set es `3732cb23...`).
  - Icono: **Social Media Logo** (set `dcce8ba288d98d8ba2677adb5081b6792a02a7e5`) — Google `89e41851f210cc135ff5d35ccb614b68d95f8ae1`, Apple `266f01f6cc5892bd33436e1bc170d49e72e68352`. Email usa `vuesax/bold/sms` (`b1c82c2fe12298fec9c42f839e57664870d6f793`).
  - Botones: "Continuar con Google", "Continuar con Apple", "Continuar con Email" (email = fallback del producto). No hay logo de Xbox en el kit; si se quiere, crear el asset.
  - Estos building blocks YA están publicados en el kit -> se pueden usar en las pantallas sin republicar.
- **Card Map Result** (set, State=Ganada/Perdida/Pendiente): `3efe31fd80f3ae2038ef84c95c38a77c1aae0843`.
  Fila BO por mapa. Capas editables: Number, Map, Mode, Score, Winner. Acento rojo en Ganada.
- **List Row** (set, Tone=Default/Danger): `e4aa7df959dd376a65456040b72b0e43563c516e`.
  Label + chevron. Danger = rojo (Cerrar sesion, etc.).
- **BottomBar TL** (set, Active=Inicio/Eventos/Equipo/Partidos/Perfil): `ab52a570bd5d3b5945f76f21e393c24e5d1a609b`.
  Reusa el BottomBar del kit; resalta en rojo la pestaña activa. Etiquetas en español.

- **Status Bar TL** (component): `eae3f0c2cbf8a23fe1559cb9dcac2fa792df031c`. 390x54. Barra de
  estado iOS (hora + señal/wifi/batería) recoloreada a blanco. La agregó Gerson (node 4407:13983
  del kit) y se convirtió a componente. Va arriba de TODAS las pantallas.

## Retrofit
HECHO (no requería publicar):
- **Login (02) y Registro (03)**: divider + SSO Auth del kit (Button Secondary + Social Media
  Logo Google/Apple + Email) con logos reales. Listo.

Pendiente (tras publicar Card Map Result, List Row, BottomBar TL y Status Bar TL):
- Todas las pantallas (11): insertar **Status Bar TL** como primer hijo (arriba del Top Nav).
- Detalle de partido (21): reemplazar las filas locales por Card Map Result.
- Perfil (40): reemplazar las filas de menú locales por List Row.
- Todas las pantallas con barra: cambiar BottomBar del kit por BottomBar TL con la variante
  Active correcta (Home->Inicio, Eventos, Mi Equipo->Equipo, Perfil).
- Imágenes reales (logos de equipo/foto de evento) en Home/Eventos/Detalle/Mi Equipo.

## Aprendizajes técnicos (kit)
- El placeholder del **Text Field** usa fuente **Outfit** (no Lexend): antes de escribir en
  cualquier nodo de texto del kit, cargar la fuente del propio nodo vía
  `getStyledTextSegments(['fontName'])` + `loadFontAsync`, si no lanza "unloaded font".
- Text Field: nodo de texto editable se llama `Text`; variantes Type=Default/Password/Phone Number/Code × State=Empty/Active/Filled.
- Button: variante default `Type=Primary, Style=Default, Size=Default, State=Default`; el label es un TEXT interno (Lexend); trae ícono y flecha.
- LogoMark importado renderiza como ícono de app rojo con el monograma (transparente, sirve como app-icon).

## Pendientes / notas
- La Home v1 (armada antes del recolor) quedó verde; requiere aceptar "Update available"
  de la librería en Temp League para tomar el rojo.
- **Social login** (Google/Apple/Xbox) es prioridad de producto pero el kit no trae botones
  sociales: hay que CREARLOS en PlayPals (Button Secondary + slot de ícono) y republicar
  antes de añadirlos a Login/Registro.
- Avatar default del kit es cuadrado-redondeado (no círculo); si se quiere circular, usar
  otra variante o crear una en PlayPals.
