# ADR-0003 · Autenticación social + perfil verificado

- **Estado:** Aceptado
- **Fecha:** 2026-08-28

## Contexto

La comunidad de Gears vive en el ecosistema Microsoft/Xbox. Se busca fricción mínima de
registro pero identidad confiable para competir (evitar cuentas falsas, poder contactar a
los jugadores). La verificación por SMS OTP como registro inicial resultaba costosa y con
fricción; conviene apoyarse en proveedores de identidad.

## Decisión

**Acceso:** login social con **Google, Apple y Xbox** + **respaldo email/contraseña**.
- De Google/Apple se obtiene correo + nombre; de **Xbox** además el **gamertag**
  (se autocompleta).
- Un usuario puede vincular varios proveedores (`authProviders[]`). El email es único.

**Perfil ("Completa tu perfil", primera vez):**
- **Teléfono** obligatorio, **verificado por SMS OTP** (usado también para contacto futuro
  vía WhatsApp/emergencias).
- **Fecha de nacimiento** obligatoria — se recolecta el dato, **sin restricción de edad**.
- **Gamertag** (autollenado si entró con Xbox).
- **Foto** y **redes sociales** opcionales (redes: plataforma de un listado + URL).

**Gating:** para **unirse a un equipo** o **inscribirse a un evento** se requiere
`perfilCompleto = true` (gamertag + teléfono verificado + fecha de nacimiento).

**Roles no excluyentes:** el rol de **dueño de organización** (nivel org) es independiente
de **jugar** (nivel equipo, máx. 1 activo) y del **rol global**. Un usuario puede ser dueño
y a la vez jugador de un roster de su propia org.

## Consecuencias

**Positivas**
- Registro de baja fricción; identidad más confiable (proveedores + teléfono verificado).
- Xbox encaja perfecto con la comunidad y aporta gamertag verificado de origen.
- SMS OTP se reduce a **una** verificación de teléfono (no a cada login) → menor costo.
- El modelo de roles resuelve el caso dueño-que-también-juega sin ambigüedad.

**Negativas / a vigilar**
- Integrar 3 proveedores OAuth (Google/Apple/Xbox) + email = más superficie de auth.
- Requisito de teléfono verificado puede frenar a algunos usuarios; se mitiga permitiendo
  navegar sin completar perfil (el gating solo aplica al competir).
- Apple Sign In tiene requisitos propios de App Store (cumplir para publicar en iOS).

## Alternativas consideradas

- **Solo SMS OTP como registro:** más fricción y costo; descartado como método principal.
- **Solo social (sin email/contraseña):** deja fuera a quien no tenga esas cuentas;
  se agrega email/contraseña como respaldo.
