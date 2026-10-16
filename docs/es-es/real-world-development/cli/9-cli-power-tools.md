---
title: "Lección 9: Comandos slash de GitHub Copilot CLI"
description: "Explora comandos slash para gestionar el contexto, elegir modelos, compartir sesiones y delegar trabajo opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye numerosos comandos slash. Estos comandos permiten acceder a funciones avanzadas, información interna y opciones de configuración adicionales. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Ahora explorarás otros que pueden resultarte útiles.

En esta lección:

- utilizarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- utilizarás `/model` para explorar los modelos disponibles;
- aprenderás cómo `/share` puede exportar o compartir una sesión;
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

## Escenario

Ya has completado el flujo de trabajo principal de CLI. Ahora explorarás otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y, opcionalmente, delegar trabajo al [agente de Copilot en la nube][about-cloud-agent].

## Explorar el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, puedes acercarte al límite de la ventana de contexto del modelo. Copilot CLI compacta la conversación automáticamente cuando hace falta; también puedes inspeccionarla o compactarla manualmente con comandos slash.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Fíjate en el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya es pequeña, puede que no haya cambiado mucho.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente cuando se llena la ventana. Utiliza `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` si vas a cambiar a una tarea no relacionada y quieres empezar una conversación nueva.

## Elegir un modelo

Los distintos modelos tienen capacidades diferentes, y cada desarrollador puede tener preferencias propias. Copilot CLI permite consultar y seleccionar el modelo que quieres usar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Colaborar con el equipo y compartir aprendizajes ayuda a todo el mundo a sacar más partido de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartirla o publicar un gist de GitHub.

1. Escribe `/help` y consulta las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir la sesión, elige el destino que mejor se adapte, como `/share file` para exportarla a un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no haya autorizado compartir.

## Opcional: ampliar el trabajo o delegarlo

El taller principal ha terminado. Copilot CLI también ofrece comandos para tareas de mayor alcance:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente de Copilot en la nube, que trabajará de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o iniciar tareas remotas. Antes de probarlos, empieza una tarea nueva y bien delimitada, y revisa el resultado con tu flujo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos slash de Copilot CLI permiten configurarlo, compartir sesiones y consultar lo que ocurre en segundo plano. En esta lección:

- has utilizado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- has utilizado `/model` para explorar los modelos disponibles;
- has aprendido cómo `/share` puede exportar o compartir una sesión;
- has explorado comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

Copilot CLI ofrece muchos más comandos slash que puedes explorar. Terminemos el recorrido [repasando lo aprendido][next-lesson] y los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Información sobre el agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
