# 07 · Spec — App de Jugadores (React Native)

La app móvil es la experiencia de **consumo** del jugador. **No** captura resultados ni
gestiona la liga (eso vive en el Admin Web). Aquí el jugador se registra, arma/gestiona su
equipo, se inscribe y sigue todo.

## Mapa de navegación

```
Splash
 └─ Bienvenida (branding)
     ├─ Continuar con Google / Apple / Xbox ─┐
     ├─ Registro con email + contraseña ─────┤
     │                                        ├─ (1ª vez) Completa tu perfil ─► [Home]
     └─ Iniciar sesión (email/contraseña) ───┘        └─ (ya completo) ──────► [Home]

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
- Botones: **Continuar con Google**, **Continuar con Apple**, **Continuar con Xbox**,
  y **Registrarme con email** / **Iniciar sesión**.

### 2. Acceso social / email
- **Social (Google/Apple/Xbox):** OAuth; traemos correo + nombre (y **gamertag** si es Xbox).
- **Email + contraseña:** registro/login clásico de respaldo.
- Aceptar términos y privacidad en el primer acceso.

### 3. Completa tu perfil (primera vez)
Se muestra la primera vez o si falta info. Campos:
- **Nickname** (obligatorio, visible en la liga).
- **Teléfono** (obligatorio) → **verificación por SMS OTP** (input de 6 dígitos, reenvío
  con cooldown). Usado también para contacto futuro (WhatsApp/emergencias).
- **Fecha de nacimiento** (obligatoria; sin restricción de edad).
- **Gamertag** (autollenado si entró con Xbox).
- **Foto de perfil** (opcional).
- **Redes sociales** (opcionales): elegir plataforma de un listado + pegar URL/usuario.
- **País** (LATAM).

> **Gating:** para **unirse a un equipo o inscribirse a un evento** el perfil debe estar
> completo (gamertag + teléfono verificado + fecha de nacimiento). Si falta algo, se
> redirige aquí antes de permitir la acción.

### 4. Iniciar sesión (retorno)
- Con el mismo método usado al registrarse (social o email/contraseña).
- Recuperar contraseña (para el método email).

### 5. 🏠 Inicio (dashboard)
- **Próximo(s) partido(s)** de mi equipo (rival, fecha, canal de transmisión).
- **Eventos activos** / con inscripciones abiertas (acceso rápido).
- Estado de **mi equipo** de un vistazo.
- Accesos directos: inscribirme, ver bracket, etc.

### 6. 🏆 Eventos
- **Lista de eventos**: filtros por tipo (Temporada / Relámpago ⚡) y estado
  (Inscripciones / En curso / Finalizado). Muestra si es gratis o con cuota.
- **Detalle de evento**: info, formato, fechas, cupo, **reglamento**, **premio** (si hay),
  cuota (si aplica), equipos inscritos.
- **Bracket** (eliminación) o **Tabla de posiciones** (liga), según formato.
- **Calendario de partidos** del evento (con canal de transmisión y enlace al stream).

### 7. 🛡️ Mi Equipo
Flujos:
- **Crear organización**: nombre + tag (validación de unicidad en vivo) + logo.
- **Crear equipo/roster** dentro de la org (ej. "Main").
- **Invitar jugadores**: por búsqueda/nickname o **código de invitación**, con rol
  propuesto (jugador / suplente / coach). El invitado **acepta**.
  - Respeta reglas: "1 equipo activo" y **ventana de transferencia 72 h** (con mensajes
    claros si el jugador está bloqueado).
- **Ver roster**: miembros, capitán, roles (titular/suplente/coach).
- **Gestión (capitán)**: expulsar miembro, **transferir capitanía**.
- **Inscribir equipo a un evento** (solo capitán/dueño):
  - Elegir evento con inscripciones abiertas → confirmar → estado `PENDIENTE`.
  - Si el evento tiene **cuota**, mostrar el paso de pago (v2).
- **Estado de inscripciones**: pendiente / aprobada / rechazada.

### 8. ⚔️ Partidos
- **Mis próximos partidos**: rival, fecha/hora, evento, **canal de transmisión**.
- **Check-in**: botón para confirmar asistencia cuando abre la ventana (evita walkover).
- **Resultados**: historial con marcador de serie.
- **Detalle de partido**: equipos, ronda/jornada, **Best-of con mapas** (mapa+modo+ganador),
  marcador de serie, estado de check-in y enlace al stream.

### 9. 👤 Perfil
- Ver/editar datos (nickname, gamertag, avatar, **redes sociales**).
- Mis equipos / mi organización (si soy dueño).
- Estado de verificación (teléfono) y de perfil completo.
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
