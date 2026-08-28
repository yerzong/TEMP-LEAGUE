# 07 · Spec — App de Jugadores (React Native)

La app móvil es la experiencia de **consumo** del jugador. **No** captura resultados ni
gestiona la liga (eso vive en el Admin Web). Aquí el jugador se registra, arma/gestiona su
equipo, se inscribe y sigue todo.

## Mapa de navegación

```
Splash
 └─ Bienvenida (branding)
     ├─ Registro ──► Verificación SMS OTP ──► Completar perfil ──► [Home]
     └─ Login ─────────────────────────────────────────────────► [Home]

[Home / Tabs]
 ├─ 🏠 Inicio       (dashboard)
 ├─ 🏆 Eventos      (lista → detalle → bracket/tabla → partidos)
 ├─ 🛡️ Mi Equipo    (org, roster, inscripciones)
 ├─ ⚔️ Partidos     (mis próximos + resultados)
 └─ 👤 Perfil       (datos, ajustes, notificaciones)
```

## Pantallas — detalle

### 1. Splash + Bienvenida
- Logo Temp League, tema visual Gears/E-Day.
- CTA: **Crear cuenta** / **Iniciar sesión**.

### 2. Registro
Campos:
- **Teléfono** (obligatorio, con lada de país) → se usa para SMS OTP.
- **Contraseña** (obligatoria).
- Aceptar términos y privacidad.

> El teléfono es único; si ya existe, se ofrece iniciar sesión.

### 3. Verificación SMS OTP
- Input de **código de 6 dígitos**.
- Reenviar código (con cooldown / rate-limit).
- Al validar → continúa a completar perfil.

### 4. Completar perfil
Campos:
- **Nickname** (obligatorio, visible en la liga).
- **Gamertag** (Xbox — texto en MVP).
- **Avatar / foto de perfil** (opcional, subir imagen).
- **País** (LATAM).

### 5. Login
- Teléfono + contraseña.
- Recuperar acceso (vía OTP).

### 6. 🏠 Inicio (dashboard)
- **Próximo(s) partido(s)** de mi equipo (rival, fecha, canal de transmisión).
- **Eventos activos** / con inscripciones abiertas (acceso rápido).
- Estado de **mi equipo** de un vistazo.
- Accesos directos: inscribirme, ver bracket, etc.

### 7. 🏆 Eventos
- **Lista de eventos**: filtros por tipo (Temporada / Relámpago ⚡) y estado
  (Inscripciones / En curso / Finalizado). Muestra si es gratis o con cuota.
- **Detalle de evento**: info, formato, fechas, cupo, equipos inscritos.
- **Bracket** (eliminación) o **Tabla de posiciones** (liga), según formato.
- **Calendario de partidos** del evento (con canal de transmisión y enlace al stream).

### 8. 🛡️ Mi Equipo
Flujos:
- **Crear organización**: nombre + tag (validación de unicidad en vivo) + logo.
- **Crear equipo/roster** dentro de la org (ej. "Main").
- **Invitar jugadores**: por búsqueda/nickname o código de invitación.
  - Respeta reglas: "1 equipo activo" y **ventana de transferencia 72 h** (con mensajes
    claros si el jugador está bloqueado).
- **Ver roster**: miembros, capitán, roles.
- **Inscribir equipo a un evento** (solo capitán/dueño):
  - Elegir evento con inscripciones abiertas → confirmar → estado `PENDIENTE`.
  - Si el evento tiene **cuota**, mostrar el paso de pago (v2).
- **Estado de inscripciones**: pendiente / aprobada / rechazada.

### 9. ⚔️ Partidos
- **Mis próximos partidos**: rival, fecha/hora, evento, **canal de transmisión**.
- **Resultados**: historial de partidos jugados con marcador.
- **Detalle de partido**: equipos, ronda/jornada, marcador, enlace al stream.

### 10. 👤 Perfil
- Ver/editar datos (nickname, gamertag, avatar).
- Mis equipos / mi organización.
- **Ajustes de notificaciones** (qué push quiero recibir).
- Cerrar sesión.

## Estados y mensajes clave (UX)

- Bloqueo por **ventana de transferencia**: "Podrás cambiar de equipo en X h."
- Nombre/tag de org **ya en uso**: feedback inmediato en el formulario.
- Evento con **cuota**: badge visible "De paga" vs "Gratis".
- Partido con transmisión: botón **"Ver en vivo"** al canal correspondiente.

## Fuera de la app móvil (a propósito)

- Captura de resultados → **Admin Web** (staff/caster/admin).
- Gestión de la liga, aprobaciones, generación de brackets → **Admin Web**.

## Notas de diseño

- Tema oscuro tipo Gears (rojo/acero), tipografía con carácter (ver fase de UI/UX en Figma).
- Bottom tabs para las 5 secciones principales.
- Prioridad de contenido: que el jugador vea **su próximo partido** y **su equipo** al abrir.
