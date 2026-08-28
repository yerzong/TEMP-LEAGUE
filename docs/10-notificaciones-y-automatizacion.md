# 10 · Notificaciones y automatización (n8n)

Dos canales complementarios:
- **Push (FCM)** → directo al jugador en la app.
- **n8n** → integraciones hacia afuera (Discord, redes, email) desacopladas del backend.

## Notificaciones push (FCM)

El backend emite push ante estos eventos:

| Evento | Destinatario | Ejemplo de copy |
|--------|--------------|-----------------|
| Invitación a equipo | Jugador invitado | "Zurco Esports te invitó a su roster" |
| Inscripción aprobada | Capitán/roster | "Tu equipo fue aceptado en Temporada 1" |
| Inscripción rechazada | Capitán | "Tu inscripción no fue aprobada" |
| Partido programado | Ambos equipos | "Tu partido vs [rival] es el [fecha]" |
| Partido reprogramado | Ambos equipos | "Tu partido se movió al [nueva fecha]" |
| Rival definido (avance) | Equipo que avanza | "Tu próximo rival es [equipo]" |
| Partido por empezar | Ambos equipos | "Tu partido empieza en 30 min · Canal: Temp League TV" |
| Resultado publicado | Ambos equipos | "Resultado: [A] 2 - 1 [B]" |
| Fin de ventana de transferencia | Jugador | "Ya puedes unirte a un nuevo equipo" |

Ajustes: el jugador puede activar/desactivar categorías desde Perfil → Notificaciones.

### Infra push
- Tokens FCM guardados por dispositivo/usuario.
- Envío encolado (Redis/BullMQ) para no bloquear el request.

## Automatización con n8n

El backend **no** habla directo con Discord/redes: emite **webhooks/eventos** y n8n
orquesta. Esto mantiene el core limpio y permite cambiar integraciones sin tocar código.

```
NestJS  ──(webhook: evento.ocurrido)──►  n8n  ──►  Discord
                                              ├──►  X / Instagram / TikTok
                                              ├──►  Email
                                              └──►  Google Sheets (respaldo/registro)
```

### Flujos n8n candidatos (MVP → v2)

| Disparador (backend) | Flujo n8n |
|----------------------|-----------|
| Inscripción aprobada | Anuncio en Discord: "Nuevo equipo confirmado en [evento]" |
| Partido programado | Post/recordatorio en Discord con fecha y canal |
| Partido por empezar | Aviso en Discord "@everyone en vivo ahora → [canal]" |
| Resultado publicado | Post automático a redes con el marcador |
| Fin de jornada | (v2) Resumen de jornada generado con IA → redes |
| Nueva organización | Registro en Sheet + bienvenida |

### Contrato de webhook (borrador)
```json
POST {n8n_webhook_url}
{
  "tipo": "RESULTADO_PUBLICADO",
  "eventoId": "…",
  "partidoId": "…",
  "data": { "local": "…", "visitante": "…", "marcador": "2-1", "canal": "Temp League TV" },
  "ts": "2026-10-06T02:00:00Z"
}
```

## Por qué n8n (y no integrar todo en el backend)
- **Desacople**: cambiar de red social o de flujo no requiere deploy del backend.
- **Velocidad**: armar/editar automatizaciones visualmente.
- **Menos código que mantener** en el core para tareas de integración.

> Regla: el backend es la **fuente de verdad y disparador**; n8n es el **orquestador de
> integraciones externas**. Nada de lógica de negocio crítica vive en n8n.
