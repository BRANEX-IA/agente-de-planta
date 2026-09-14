# Agente de planta · Cloudflare Workers

[![Deploy to Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/BRANEX-IA/agente-de-planta)

Agente de IA para el taller del profe Jefferson. Conversa en español, consulta el cierre de una planta de confección con
**datos SIMULADOS** y **pide firma** antes de hacer cualquier cosa que no sea leer. Vive en Cloudflare Workers y se usa
desde el navegador o el celular.

> Basado en la plantilla oficial [`cloudflare/agents-starter`](https://github.com/cloudflare/agents-starter) (licencia MIT,
> ver `LICENSE`), tomada el 2026-09-14 (commit `4ea6a72`). Branex le entrega al profe la guía paso a paso por aparte.

## Qué cambió frente a la plantilla oficial

| Pieza              | Plantilla oficial                                 | Este agente                                                                                                                                                                                                             |
| ------------------ | ------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Cerebro            | `@cf/moonshotai/kimi-k2.7-code` (solo plan pago)  | `@cf/zai-org/glm-4.7-flash` (plan gratis, sabe usar herramientas)                                                                                                                                                       |
| Manos              | Clima al azar, calculadora, zona horaria          | `cierreDePlanta`, `novedadesDelTurno` y `verTareasProgramadas` solo leen. Todo lo demás (`avisarAlJefeDePlanta`, `programarTarea`, `cancelarTareaProgramada`) pide firma, porque `conSupervisor` lo aplica en el código |
| Tareas con horario | Aviso que solo se ve si la pestaña está abierta   | El resultado queda **escrito en el chat**. Los horarios cron van en UTC (Colombia es UTC-5)                                                                                                                             |
| Servidores MCP     | Cualquiera con la dirección podía conectar uno    | Quitados                                                                                                                                                                                                                |
| Acceso             | Abierto                                           | **Cerrado por defecto:** pantalla de entrada con clave y una cookie `HttpOnly` que guarda la firma HMAC de la clave, no la clave                                                                                        |
| Pantalla           | Inglés, con adjuntos, depuración y cambio de tema | Español, solo texto, nombres legibles de las manos y encabezado que cabe en el celular                                                                                                                                  |

Las fórmulas y los datos simulados son los mismos del regalo 01 del taller de n8n. Dan 76,9 % de eficiencia, módulos 4 y 7
caídos, y no cumplen los módulos 3, 4, 6 y 7.

## Publicarlo sin terminal (sirve en Windows 10)

1. Botón **Deploy to Cloudflare** de arriba.
2. En la misma página se pide `CLAVE_DE_ACCESO` (sale de `.dev.vars.example`): una clave de **20 caracteres o más**. Si no la
   pide, se crea en **Workers & Pages** → el agente → **Settings** → **Variables and Secrets** → **Add**, tipo **Secret**.
3. Abrir `https://<nombre>.<subdominio>.workers.dev`, escribir la clave y dar clic en **Entrar**.

Los cambios se hacen con Claude Code en la web: rama nueva → **Create PR** → **Merge** en GitHub. Workers Builds publica
solo al actualizar `main`.

## Con terminal (Mac, Windows 11 o Linux)

```bash
npm install
cp .dev.vars.example .dev.vars   # y poner una clave de 20 caracteres o más; .dev.vars no se sube al repositorio
npx wrangler login               # Workers AI no tiene simulador local: pide una cuenta de Cloudflare
npm run dev                      # http://localhost:5173
npm run check && npm run build
npm run deploy
```

## Reglas que no se rompen

- **`SOLO_LEEN` en `src/server.ts` es la lista de manos que no piden firma.** Agregar un nombre ahí es quitarle el
  supervisor: lo decide el dueño del agente, nunca un asistente de código.
- **La función `fetch` del final de `src/server.ts` es la puerta.** Sin clave de 20 caracteres o más responde 503. Sin sesión,
  o si la conexión viene de otro sitio, responde 401.
- **Las tareas programadas solo leen** y dejan el resultado escrito; nunca envían nada.
- **Nada de llaves ni datos personales en el código.** Los secretos van en Cloudflare con tipo **Secret** y se leen de
  `this.env`.
- **La clave no es un inicio de sesión con usuarios.** Para datos reales hace falta más protección: Cloudflare Access con
  dominio propio u otra opción. Antes, pasar por el semáforo y tener la autorización de la empresa.

## Deuda conocida

- La plantilla fija versiones anteriores a las últimas: `agents` 0.17.4 frente a 0.23.0, `ai` 6 frente a 7 y
  `workers-ai-provider` 3 frente a 4. Subirlas es un cambio mayor que hay que probar aparte.
- No se ha publicado en una cuenta real ni se ha probado con el modelo.
