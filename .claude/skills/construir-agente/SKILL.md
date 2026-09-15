---
name: construir-agente
description: Paso a paso para convertir esta plantilla en el agente propio del dueño y enseñarle cosas nuevas (manos que leen, manos que actúan, tareas con horario, sus datos, formatos de correo) sin romper la clave ni la firma. Úsela cuando el dueño quiera crear su agente, agregarle algo, conectar datos o cambiar cómo se comporta.
argument-hint: "[qué quiere que haga su agente]"
---

# Construir su agente

Usted ayuda a una persona que **no programa** a construir su agente de IA sobre esta plantilla (Cloudflare Workers con el
Agents SDK). Háblele **de usted**, en español de Colombia, con palabras simples y **una pregunta a la vez**. No le muestre
código salvo que lo pida: cuéntele en palabras qué va a cambiar.

## 1. Antes de tocar nada

1. Lea `CLAUDE.md` de la raíz: son las reglas que no se rompen.
2. Si no están instaladas las dependencias, corra `npm install`.
3. Si el dueño no dijo qué quiere, pregúntele con estas opciones y siga la guía que corresponda:

| Quiere que su agente…                                           | Guía                                                                  |
| --------------------------------------------------------------- | --------------------------------------------------------------------- |
| Deje de ser el agente de planta y sea SU agente                 | [1-mi-agente.md](1-mi-agente.md)                                      |
| Consulte algo nuevo (mano que lee)                              | [2-mano-que-lee.md](2-mano-que-lee.md)                                |
| Haga algo nuevo (mano que actúa)                                | [3-mano-que-actua.md](3-mano-que-actua.md)                            |
| Trabaje solo a una hora                                         | [4-tarea-con-horario.md](4-tarea-con-horario.md)                      |
| Lea sus propios datos: una hoja, una base de datos o documentos | [5-sus-datos.md](5-sus-datos.md)                                      |
| Mande un tipo de correo nuevo                                   | [6-formato-de-correo.md](6-formato-de-correo.md)                      |
| Se comporte distinto                                            | Cambie solo `INSTRUCCIONES` en `src/server.ts` y cierre con el paso 3 |

4. Pregunte siempre **de qué color son los datos** que va a tocar (semáforo, regla 9 de `CLAUDE.md`). Si son rojos,
   deténgase y explíquele por qué.

## 2. Mientras construye

- Antes de cambiar código, muéstrele un **plan en palabras** de máximo 8 líneas y espere su «sí».
- Lo que ya trae la plantilla y se puede reusar: la firma en la pantalla y en Telegram, `enviarCorreo` con sus formatos en
  `src/correo.ts`, `programarTarea` y `executeTask`, y los datos simulados como ejemplo.
- Si necesita un detalle de Cloudflare (bindings, D1, AI Search), consulte la documentación oficial vigente en
  developers.cloudflare.com antes de escribirlo: cambia seguido.

## 3. Al terminar, siempre

1. Corra `npm run check` y `npm run build`. Si algo falla, arréglelo y vuelva a correrlos.
2. Deje el **recibo** de [recibo.md](recibo.md), exacto y con la verdad.
3. Explíquele en 3 líneas qué cambió y cómo probarlo. Recuérdele el camino: **Create PR → revisar el recibo → Merge pull
   request**. Si en el recibo algo dice «cambiada» o «cambiados», o hay un nombre nuevo en `SOLO_LEEN`, dígale que **no
   haga merge** y que lo revise con Branex.
