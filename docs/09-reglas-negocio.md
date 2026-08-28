# 09 · Reglas de negocio y flujos

Reglas que gobiernan cómo se comporta la plataforma. Son la fuente de verdad para el
backend y para el diseño de UX.

## Roles

| Rol | Alcance | Dónde opera |
|-----|---------|-------------|
| **PLAYER** | Jugar: perfil, equipo, inscripciones, ver eventos | App móvil |
| **STAFF / CASTER** | Transmitir y **capturar resultados**, programar partidos | Admin Web |
| **ADMIN** | Control total: eventos, aprobaciones, canales, usuarios | Admin Web |

> Roles a nivel equipo (independientes del rol global): **CAPITAN** y **JUGADOR**.

## Cuentas y acceso

### Métodos de acceso
- **Login social:** Google, Apple y **Xbox**. Un mismo usuario puede vincular varios.
- **Respaldo:** registro con **email + contraseña**.
- De Google/Apple llega **correo + nombre**; de **Xbox** además el **gamertag**
  (se autocompleta). El email es **único** por cuenta.

### "Completa tu perfil" (primera vez)
Tras el primer acceso, el usuario completa:
- **Teléfono** (obligatorio) → verificación por **SMS OTP** · usado también para contacto
  futuro (WhatsApp / emergencias).
- **Fecha de nacimiento** (obligatoria) → se guarda la fecha, **sin restricción de edad**
  (solo se recolecta el dato).
- **Gamertag** (autollenado si entró con Xbox).
- **Foto de perfil** (opcional).
- **Redes sociales** (opcionales): se eligen de un listado (Twitter/X, Facebook, Instagram,
  Twitch, YouTube, TikTok…) + URL/usuario. Varias permitidas.

### OTP
- Código de 6 dígitos, expira (TTL en Redis), con rate-limit de reenvío.
- El teléfono es **único** (un teléfono = una cuenta).

## Roles no excluyentes (dueño que también juega)

Un mismo usuario puede llevar **varios "sombreros" a la vez**:

| Sombrero | Nivel | ¿Implica jugar? |
|----------|-------|-----------------|
| **Dueño de organización** | Org (marca) | ❌ No — administra la marca y sus rosters |
| **Capitán / Jugador** | Equipo/Roster | ✅ Sí — compite (**máx. 1 equipo activo**) |
| **Rol global** (PLAYER/STAFF/ADMIN) | Plataforma | Permisos del sistema |

- Ser **dueño no consume** el cupo de "1 equipo activo"; **jugar sí**.
- Un usuario puede ser dueño de "Zurco Esports" **y** jugar en su roster "Zurco Main"
  (su único equipo activo), mientras otros rosters de la org los juegan otras personas.
- Para jugar en otro roster aplican las reglas normales (1 equipo activo + ventana 72 h).

## Requisitos para competir (gating)

- **Solo navegar/ver:** basta con tener cuenta.
- **Unirse a un equipo o inscribirse a un evento:** requiere **perfil completo mínimo** →
  **gamertag + teléfono verificado + fecha de nacimiento**. Si falta algo, la app envía a
  "Completa tu perfil" antes de permitir la acción (`perfilCompleto = true`).
- Foto de perfil y redes sociales **nunca** son requisito.

## Organizaciones, equipos y membresía

### Unicidad de marca
- `nombre` y `tag` de organización son **únicos** (case-insensitive), validación
  **automática** al crear. No hay dos marcas idénticas. Sin aprobación manual (MVP).

### Jerarquía
- Un **Usuario** puede ser **dueño** de una organización.
- Una **Organización** tiene uno o varios **Equipos/Rosters** (main, academy, etc.).
- Un **Equipo** tiene un **capitán** y varios **jugadores**.

### Un jugador = un equipo activo
- Un jugador solo puede pertenecer a **un equipo activo** a la vez.
- No puede estar en dos rosters simultáneamente (ni de la misma ni de distinta org).

### Ventana de transferencia (72 h)
- Al **unirse o salir** de un equipo, el jugador queda **bloqueado 72 h**
  (`transferLockUntil`) antes de poder unirse a otro.
- Objetivo: evitar el "brinca-brinca" de rosters, sobre todo cerca o durante eventos.
- Excepción: la primera vez que un jugador se une a un equipo puede ir sin bloqueo previo
  (el bloqueo se activa **al salir**). *(Regla afinar en implementación.)*

### Capitán / cambios de roster
- Solo el **capitán** (o dueño) puede inscribir el equipo a un evento.
- Cambios de roster de un equipo **ya inscrito y en curso** en un evento pueden bloquearse
  según el evento (roster congelado al iniciar). *(Política por evento, v2.)*

## Eventos

### Ciclo de vida
```
BORRADOR ──► INSCRIPCIONES ──► EN_CURSO ──► FINALIZADO
   (admin)      (admin abre)     (admin inicia:      (admin cierra)
                                  genera bracket/tabla)
```

### Cuota de entrada (gratis por default)
- Cada evento tiene `cuotaEntrada`. Si es **0 → gratis** (caso normal MVP).
- Si es **> 0 → la inscripción requiere pago** (`pagoEstado`); solo entra al bracket
  cuando está `PAGADO`. La pasarela real (MercadoPago/Stripe) es **v2**; el modelo ya lo
  soporta.

### Inscripción
- El capitán inscribe su roster → estado `PENDIENTE`.
- El admin **aprueba/rechaza**. Si el evento tiene cuota, además debe quedar `PAGADO`.
- Solo inscripciones `APROBADA` (+`PAGADO` si aplica) participan.

## Partidos y resultados

### Captura de resultados (importante)
- **Nadie sube su propio resultado desde la app.** Siempre hay transmisión oficial.
- El **staff/caster/admin** captura el marcador **desde el Admin Web** → se guarda
  `reportadoPorId` y `canalTransmisionId`.
- Al marcar el partido `FINALIZADO`:
  - **Eliminación:** el ganador avanza a `siguientePartidoId`.
  - **Liga:** se recalcula la tabla de posiciones.
- **Sin disputa:** lo capturado por el staff es **definitivo**; solo un **admin** puede
  **corregir** un error de captura.
- **Best-of por mapa · check-in · walkover:** ver detalle en
  [12-formato-partido-y-dia-de-partido.md](./12-formato-partido-y-dia-de-partido.md).

### Transmisiones
- La liga tiene **varios canales** (Twitch/YouTube). Cada partido se asigna a un canal.
- Un caster puede estar asignado a transmitir/reportar en un canal específico.

## Flujos principales (resumen)

### A) Onboarding de jugador
```
Abrir app → Registro (teléfono) → SMS OTP → Completar perfil (nickname/gamertag/avatar)
→ Home
```

### B) Crear organización e inscribir a evento
```
Jugador → Crear Organización (nombre+tag únicos) → Crear Equipo/Roster
→ Invitar jugadores (respetando "1 equipo activo" + ventana 72h)
→ Elegir evento con inscripciones abiertas → Inscribir roster
→ (Admin aprueba) [→ pago si hay cuota] → Roster dentro del evento
```

### C) Día de partido
```
Admin/Staff programa partido + asigna canal
→ Caster transmite en el canal de la liga
→ Caster/Staff captura marcador en Admin Web → FINALIZADO
→ Sistema avanza bracket / recalcula tabla
→ Notificación push a los equipos + n8n publica en Discord/redes
```

## Notificaciones (qué dispara push)

Ver detalle en [10-notificaciones-y-automatizacion.md](./10-notificaciones-y-automatizacion.md).
Eventos que notifican: invitación a equipo, inscripción aprobada/rechazada, partido
programado, "tu partido empieza pronto", resultado publicado, fin de ventana de
transferencia.

## Resumen de invariantes (para tests)

- [ ] Email único por cuenta; teléfono único y verificado por OTP.
- [ ] No se permite unirse a equipo / inscribirse sin `perfilCompleto = true`.
- [ ] Nombre/tag de org únicos (case-insensitive).
- [ ] Un usuario nunca en dos equipos activos.
- [ ] No unirse a un equipo si `transferLockUntil` está en el futuro.
- [ ] Solo `APROBADA` (+`PAGADO` si cuota) entra al bracket/tabla.
- [ ] Resultado solo capturable por `STAFF`/`ADMIN`.
- [ ] Cerrar partido de eliminación propaga el ganador exactamente una vez.
