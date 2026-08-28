# 13 · Cuenta, legal y moderación

Cubre la gestión de la cuenta, lo legal (datos personales) y la moderación de conducta
(bitácora antideportiva, sanciones y reportes).

## Cuenta y legal

- **Aviso de privacidad + Términos y condiciones:** obligatorios en el primer acceso
  (aceptación registrada). Se recolectan datos personales (teléfono, fecha de nacimiento),
  por lo que aplica la normativa de datos personales (México/LATAM).
- **Borrar cuenta:** el usuario puede solicitar eliminar su cuenta y datos desde Perfil.
  - Se anonimiza/borra su información personal; sus resultados históricos en eventos se
    conservan de forma anonimizada por integridad de las competencias.
- **Zona horaria:** las fechas se guardan en **UTC** y se muestran en la **hora local** de
  cada usuario (comunidad de varios países).
- **Idioma:** español (LATAM) en el MVP.

## Staff y multi-admin

- **Todo el staff opera en la Admin Web** (la app móvil es solo de jugadores).
- Roles de plataforma: `ADMIN` (control total) y `STAFF/CASTER` (transmitir + capturar
  resultados; asignable a canales/partidos).
- **Alta de staff:** un `ADMIN` **invita por correo** desde la Admin Web y asigna el rol.
- **Log de auditoría:** se registran acciones sensibles (capturar/corregir resultado,
  aprobar inscripción, mover fecha, emitir sanción) con autor y fecha.

## Moderación: bitácora de conducta

Cada usuario tiene un **historial de conducta** visible para el admin en el detalle de
usuario. Combina **reportes** (denuncias) y **sanciones** (acciones del admin).

### Reportes (denuncias)
```
Jugador reporta conducta antideportiva (motivo + descripción + evidencia opcional)
  → Reporte ABIERTO
  → Admin revisa (EN_REVISION)
  → Resuelve: aplica sanción (RESUELTO) o descarta (DESCARTADO)
```

### Sanciones
Tipos (`TipoSancion`):
- **ADVERTENCIA** — queda registrada, sin bloqueo.
- **SUSPENSION_TEMPORAL** — con `fechaFin`; bloquea competir mientras esté activa.
- **EXPULSION_EVENTO** — saca al usuario/equipo de un evento específico.
- **BANEO_PERMANENTE** — bloquea competir indefinidamente.

Cada sanción guarda: motivo, **severidad** (`LEVE`/`MEDIA`/`GRAVE`), evento (si aplica),
quién la emitió y vigencia.

### Efecto en el gating
- Una sanción **activa** de tipo `SUSPENSION_TEMPORAL` o `BANEO_PERMANENTE` **impide**
  unirse a equipos e inscribirse/competir (se suma a los requisitos de perfil completo).
- `EXPULSION_EVENTO` retira la inscripción del evento indicado.

## Flujo resumido

```
Reporte (jugador) ──► Admin revisa ──► Sanción (opcional) ──► Bitácora del usuario
                                              │
                                              └─► si activa (susp./baneo) ──► bloquea competir
```

## Invariantes (para tests)

- [ ] Aceptación de términos/privacidad registrada antes de operar.
- [ ] Usuario con sanción activa (suspensión/baneo) no puede unirse a equipo ni inscribirse.
- [ ] Solo `ADMIN` emite/anula sanciones y resuelve reportes.
- [ ] Acciones sensibles quedan en el log de auditoría con autor y fecha.
- [ ] Borrar cuenta anonimiza datos personales conservando integridad de resultados.
