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

HECHO (tras publicar los 4 componentes):
- **Status Bar TL** insertada arriba en las 10 pantallas de contenido (Onboarding, Login,
  Registro, OTP, Completa perfil, Home, Eventos, Detalle, Mi Equipo, Perfil). Splash no lleva.
  Se restó 54px al paddingTop de las pantallas de auth para mantener el espaciado.
- **BottomBar TL** con Active correcta en Home (Inicio), Eventos, Mi Equipo (Equipo), Perfil.
- **Detalle de partido**: filas de mapa -> instancias de **Card Map Result**.
- **Perfil**: menú -> instancias de **List Row** (Ajustes Default, Cerrar sesion Danger).

## Pantallas adicionales (build completo de la app)
Cada flujo en su propia página. Todas 390x844, Status Bar TL arriba, contenido Gears.
- **App · Notificaciones** — `50 · Notificaciones`: Card Notification (Invitation/Competition/
  Announcement/Badge) con Aceptar/Rechazar, contexto Gears.
- **App · Detalle de evento** — `22 · Detalle de evento`: hero con foto + chip, tabla de info
  (fecha/formato/cupo/premio/entrada), descripción, botón "Inscribir a mi equipo".
- **App · Inscripcion** — `23 · Inscripcion`: card de equipo + selección de roster (fotos +
  checkbox rojo, 5 titulares/1 suplente), "Confirmar inscripcion".
- **App · Check-in** — `24 · Check-in`: header de partido (crestas), contador 12:34, estado de
  roster (Listo/Pendiente), botón "Hacer check-in".
- **App · Tabla de posiciones** — `60`: header de columnas + 8 filas con crest-monograma,
  NOVA resaltada.
- **App · Playoffs** — `61`: Card Bracket por rondas (Cuartos/Semis/Final).
- **App · Ranking** — `62`: pills (Equipos/Jugadores/Regiones) + secciones por Tier con filas
  ranked y puntos.
- **App · Salon de la Fama** — `63`: campeón vigente (card gradiente) + campeones anteriores.
- **App · Crear equipo** — `31`: subida de logo, Nombre/Tag (nota de unicidad)/Region
  (Dropdown del kit), "Crear equipo".
- **App · Ajustes** — `41`: List Row agrupadas (Cuenta/Notificaciones con toggles/Preferencias/
  Legal) + Cerrar sesion (Danger).
- **App · Historial** — `42`: pills (Todos/Ganados/Perdidos) + Card Match con crestas reales
  por rival (NOVA vs Halcones/Azteca/Reapers/Kraken/Lobos) y resultado.

Inventario total: **22 pantallas** (6 auth + 5 core + 4 eventos + 4 competencia + 3 gestión).

## Imágenes reales (HECHO)
Se reemplazaron las imágenes de muestra del kit por assets reales (subidos vía upload_assets,
guardados en `~/Downloads/tl-assets/`):
- **Crestas de equipo** (DiceBear initials PNG 240px): NOVA (`82be00f3…`, monograma NE) y
  AZTECA (`7f9af0fc…`, NE roja). Aplicadas en Card Match de Home y Detalle, y en la cabecera
  de Mi Equipo. Técnica: ocultar el `Club Avatar` de muestra (vectores), redondear el frame
  `Avatar` (cornerRadius 999 + clip) y poner la cresta como IMAGE fill.
- **Fotos de evento** (esports/gaming, Unsplash): `59c99722…`, `0c1f13d4…`, `0e2b8b30…`.
  Aplicadas en Card Competition de Home y rotadas en las 4 de Eventos.
- **Fotos de jugadores** (randomuser.me): 5 hashes aplicados a los 6 Card Player del roster.
- **Crestas de rivales** (DiceBear): Halcones `8f3a641e…`, Kraken `f7d08464…`, Reapers
  `762cc981…`, Lobos MX `d4a2567d…` — usadas en Historial. Hashes NOVA `82be00f3…`,
  AZTECA `7f9af0fc…`. Fotos de evento `59c99722…`/`0c1f13d4…`/`0e2b8b30…`.

Aprendizaje: al **republicar** la librería, las instancias de componentes del kit con
imágenes (p.ej. avatar de Card Player) pierden la imagen (los image fills no viajan entre
archivos). Solución: reasignar un IMAGE fill propio (subido a este archivo). El avatar de
Perfil (otra variante) sí conservó su imagen.

upload_assets NO puede apuntar a sublayers de instancia (su `nodeId` sólo acepta `\d+[:-]\d+`).
Workaround: subir sin nodeId (crea frames temporales), leer el `imageHash` y aplicarlo con
use_figma a los nodos objetivo (`fills=[{type:'IMAGE',scaleMode:'FILL',imageHash}]`), luego
borrar los frames temporales.

Pendiente:
- (Opcional) Logo de Xbox como asset si se quiere SSO Xbox en vez de Email.
- (Opcional) Crestas para más equipos en tabla/eventos si se agregan.

## Correcciones de validacion (2026-08-30, feedback de Gerson)
- **Status Bar TL con fondo blanco**: el componente/instancias tenian fill blanco solido -> se
  veia un bloque blanco arriba. Fix: fill del componente en el kit a transparente (`fills=[]`) y
  override de las 21 instancias en Temp League a transparente. Iconos ya en blanco (contrastan
  sobre el fondo acero).
- **Logo descentrado en Splash/Onboarding**: al redimensionar el LogoMark con `resize()` el
  glifo interno (`logo temp`) NO escala (queda 58x34 pegado arriba-izquierda). Fix: recrear la
  instancia y usar `node.rescale(factor)` (escala el nodo Y sus hijos), no `resize()`. En
  Onboarding ademas se quito el fill del LogoMark para que el glifo quede sobre el panel gradiente.
- **Boton atras con icono equivocado**: el IconButton por defecto trae `vuesax/bold/image`. Fix:
  `setProperties({'Icon#5588:177': arrowLeftComp.id})` con `vuesax/outline/arrow-left`
  (`be32bc7b11176c493e4ec72422c6aa7a77495f9f`). Aplicado en Registro/OTP/Completa perfil.

## Auth v2 — Welcome + Login (2026-08-30, a gusto de Gerson)
Página **App · Auth v2 (Welcome + Login)** (node 73:222), inspirada en los flujos WELCOME
(kit 336:10751) y LOGIN (kit 391:19670) que le gustaron a Gerson, adaptados a Temp League:
- **W1/W2/W3** — carrusel de onboarding: imagen esports grande (redondeada), "Saltar", dots,
  título + subtítulo, botón Continuar/Empezar. (Encuentra tu liga / Arma tu escuadra / Salon de la Fama).
- **L0 · Login (SSO)** — logo + TEMP LEAGUE + 3 botones sociales (Google/Apple/Email), estilo del
  Login del kit que prefirió.
- **L1 · Login correo (vacio)** y **L2 · Login correo (lleno)** — el "Login with email": header,
  labels (Correo/Contraseña), Text Field, checkbox "Mantener sesion", "Olvidaste tu contraseña",
  botón Iniciar sesion, footer Registrate. Dos estados: **placeholder** vs **inputs llenos**
  (correo escrito, password en dots, checkbox marcado) usando variantes State=Empty/Filled del Text Field.
- **R1/R2 · Registro (vacio/lleno)** — nombre, correo, teléfono, contraseña + términos + Crear cuenta
  + "o registrate con" (Google/Apple) + footer Inicia sesion. Lleno: datos + password en dots
  (el campo Phone Filled requiere limpiar el nodo de formato "(000)…" y poner el número).
- **P1/P2 · Completa perfil (vacio/lleno)** — avatar (placeholder punteado vs foto real) + badge,
  Nombre para mostrar, Gamertag de Xbox, Region, botón Entrar a Temp League.
- **F1/F2/F3 · Recuperar contraseña** — correo → código (4 Text Field Type=Code + reenvío) →
  nueva contraseña + confirmar. (Como el flujo Forgot Password del kit.)

**Reestructura del flujo auth/onboarding (2026-08-30, decision Gerson):**
Se separo AUTENTICACION (varia por metodo) del ONBOARDING (igual para todos), porque el
Registro duplicaba datos con Completa Perfil y no aplicaba a los usuarios sociales.
- **Autenticacion**: Social = 1 toque (Google/Apple/Xbox, SIN contraseña); Correo = correo +
  contraseña + confirmar (SOLO eso — se quitaron nombre y telefono del Registro).
- **Onboarding (para TODOS los metodos)**:
  1. **Verificar telefono** (paso propio): V1/V1b numero -> V2/V2b codigo SMS OTP. Obligatorio
     para social y correo (anti-duplicados).
  2. **Completa perfil**: nombre, gamertag Xbox, region, foto (nombre/foto se pre-llenan si es social).
- La **contraseña** solo existe en el flujo por correo; los sociales no tienen contraseña.
- Nueva **seccion 4 · Verificar telefono**; secciones renumeradas en orden de flujo (1 Welcome,
  2 Login, 3 Registro, 4 Verificar telefono, 5 Completa perfil, 6 Recuperar, 7 Legal).
- Prototipo recableado (27 links): Social->Verificar telefono; Correo->Login o Registro->Verificar
  telefono; Verificar->OTP->Completa perfil->Cargando.

**Legal + Recuperar llenos + prototipo (2026-08-30):**
- **F1b/F2b/F3b · Recuperar (llenos)** — versiones con datos: correo escrito, codigo 8-2-4-1
  (Text Field Type=Code State=Filled), contraseñas en `●`. Fila 2 de la seccion Recuperar.
- **Seccion 6 · Legal**: **LG1 · Terminos y Condiciones** y **LG2 · Aviso de Privacidad** con
  contenido real de Temp League (aceptacion, cuentas, roster, resultados staff-final, conducta/
  sanciones, datos ARCO, terceros Google/Apple/Xbox/FCM, etc.).
- **Prototipo Auth v2 (19 links, misma pagina)**: Welcome->Login->formularios->Recuperar. Accesos:
  - **Recuperar contraseña**: Login "Olvidaste tu contraseña?" -> F1 -> F2 (codigo) -> F3 (nueva) -> Login.
  - **Terminos/Privacidad**: texto "Al continuar aceptas..." en L0/R1/R2 -> LG1 (Terminos). En
    Ajustes las filas "Terminos"/"Privacidad" apuntan a LG1/LG2 (navegacion del producto).
  - **Limite Figma**: NAVIGATE solo funciona entre frames de la MISMA pagina. Por eso el link de
    Ajustes (otra pagina) a Legal no se puede cablear como prototipo; queda en el flujo documentado.
    Para un prototipo 100% clickeable de toda la app habria que consolidar en una pagina o usar
    Code Connect / el routing real de la app.

**Ajustes de validacion (Auth v2):**
- **Splash** (S0) y **Cargando** (L3) agregados: logo + spinner `Loading Style=Primary`
  (`f177a342d5412fbdd766651a60e9c3a10154fd09`) + version / "Configurando tu cuenta...".
- **Checkbox**: se reemplazo el checkbox custom por el **componente Checkbox del kit**
  (set `f5ecd85cbdd080a0ac971fa49110424ad5da3e27`; True `9606eaaa…`, False `a2dabd6f…`) en el
  Login (Mantener sesion) y en el roster de Inscripcion (6). Regla: usar el componente Checkbox
  en todo el proyecto.
- **Contraseña**: los inputs llenos usan `●` (U+25CF, circulos negros) en vez de `•` (bullets
  chiquitos que se veian mal). Variante Text Field Type=Password State=Filled.
- **Espaciado**: en el Login se movieron las opciones (checkbox/olvidaste) dentro del Form y se
  puso gap **32** en Content (como el kit) para que el footer "No tienes cuenta" respire.
- **Subida de foto de perfil** (Completa perfil): avatar 112px con icono de camara
  (`vuesax/bold/camera` `9f4756bc…`), badge rojo con camara y texto "Agregar foto de perfil" /
  "Cambiar foto". Empty = circulo punteado con camara; Filled = foto real.
- **Secciones**: la pagina Auth v2 se organizo en 5 **Figma Sections** (1 Welcome, 2 Login,
  3 Registro, 4 Completa perfil, 5 Recuperar contrasena), dimensionadas para envolver cada flujo
  (excepcion a la regla de "no usar sections": aqui Gerson las pidio y quedan limpias).

**Nota (decision Gerson):** Auth v2 es el look OFICIAL de acceso. NO cambia la lógica/flujo del
producto (definido en `/docs`): SSO Google/Apple + correo, verificación por SMS OTP, completar
perfil con gamertag, recuperación de contraseña. La página vieja "App · Onboarding + Auth" queda
como referencia previa (se puede archivar cuando Gerson confirme).

## Correcciones de validacion (2da ronda, 2026-08-30)
- **"+" en todos los botones**: era el `Icon Left` (add-circle) del Button del kit. Fix:
  `setProperties({'Has Icon Left#5588:152': false})` en todos los Button NO-SSO (11 botones).
  Los SSO se saltan (ahi el Icon Left es el logo de marca).
- **Logo de Google con fondo blanco**: el instance de Social Media Logo (Google) tiene fill
  blanco propio + 2 vectores quedaron en gris por el recolor previo. Fix: `fills=[]` en el
  instance (transparente) + recolorear los 2 grises a azul (66,133,244) y verde (52,168,83)
  de Google. (Login y Registro.)
- **Boton atras (estilo)**: se cambio el IconButton rojo por una **flecha simple** blanca
  (`vuesax/outline/arrow-left`) al estilo del Top Nav del kit (node 295:6922 = Top Nav con
  LeadingItem Type=Icon + CenterItem Type=Text). Aplicado en Registro/OTP/Completa perfil.
- **Pantallas completas (sin recorte)**: cada frame se fijo a `max(844, alturaContenido)` con
  `clipsContent=false`; el contenedor de scroll con `layoutGrow=1` para anclar barras/botones
  abajo. Splash centrado y Onboarding con hero que llena. Las pantallas con mucho contenido
  crecen verticalmente (Eventos 1420, Playoffs 1041, Mi Equipo 988, Home 969, Historial 956).

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
