---
title: "Ejercicio 3: Probar la función con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de desarrollar la función de filtrado con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En lugar de probar la aplicación por tu cuenta, conectarás el **servidor MCP de Playwright** y dejarás que Copilot controle un navegador real para probar la función. Después, publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas disponibles para Copilot;
- añadirás el servidor MCP de Playwright al espacio de trabajo;
- pedirás a Copilot que lo utilice para probar manualmente la función de filtrado en un navegador;
- publicarás la rama y abrirás una solicitud de incorporación de cambios para el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede acceder a herramientas ajenas al editor. Copilot utiliza Model Context Protocol (MCP), una forma estándar de comunicarse con herramientas y servicios externos.

![Diagrama que muestra los componentes internos del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden consultar información actualizada (mediante recursos) y realizar acciones en tu nombre (mediante herramientas).

Los agentes acceden a estas herramientas y recursos mediante un servidor MCP, que actúa como puente entre el agente y las herramientas externas (por ejemplo, API existentes o herramientas locales como paquetes de NPM). Cada servidor MCP ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a API para gestionar repositorios de GitHub y permite crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: ofrece funciones de automatización del navegador con Playwright, como abrir páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP disponibles, cada uno con herramientas y recursos diferentes. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Por seguridad, trata los servidores MCP como cualquier otra dependencia. Antes de utilizar uno, revisa detenidamente su código fuente, verifica quién lo publica y evalúa las implicaciones de seguridad. Utiliza solo servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el codespace.
2. Deberías encontrar un servidor `github` ya configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto incluye la entrada `github`, lo que ha permitido a Copilot leer el backlog de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hay nada que instalar localmente. VS Code se conecta mediante HTTP y te autentica con GitHub la primera vez que Copilot utiliza una de sus herramientas.

## Añadir el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador controlable para probar la función.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es local: VS Code lo inicia en el equipo con `npx @playwright/mcp@latest`. La opción `--headless` hace que Playwright ejecute el navegador sin mostrar una ventana, algo necesario en un codespace, donde no hay un escritorio para verla.

> [!NOTE]
> Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, por lo que normalmente ya está instalado el navegador que necesita. Si Copilot indica que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Iniciar el servidor y confirmar que es de confianza

VS Code inicia los servidores MCP **bajo demanda**: la primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json` aparecerá una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para poder iniciarlo.

Este inicio manual es opcional. Si lo omites, VS Code iniciará el servidor y te pedirá que confirmes que confías en él cuando Copilot utilice sus herramientas en el paso siguiente.

## Probar la función de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real que puede controlar. En lugar de probar la aplicación por tu cuenta, el agente puede abrir una página, navegar, aplicar filtros y leer el resultado para resumirte lo que ha observado. Es una forma rápida de comprobar el comportamiento de una función sin salir de la conversación.

Internamente, el servidor MCP de Playwright utiliza el [árbol de accesibilidad][playwright-mcp-server] de la página en vez de capturas de pantalla. El agente trabaja con elementos estructurados y etiquetados, como botones, enlaces y elementos de listas, del mismo modo que una tecnología de asistencia. Por eso, una comprobación funcional rápida también sirve como verificación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la función de filtrado que acabas de desarrollar:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te indicará lo que ha observado. Compara el resumen con los criterios de aceptación de la incidencia. Si algo no funciona como debería, haz preguntas de seguimiento o pide a Copilot que corrija el código antes de abrir una solicitud de incorporación de cambios.

> [!NOTE]
> La aplicación debe estar disponible en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot utilice el servidor MCP de Playwright, puede que tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una solicitud de incorporación de cambios

Ya has confirmado que la función funciona y puedes abrir una solicitud de incorporación de cambios (PR) para que el equipo la revise. Primero, publicarás la rama `filtering-vscode`.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crear la solicitud de incorporación de cambios

Hay varias formas de crear una solicitud de incorporación de cambios: desde github.com, con la interfaz de línea de comandos de GitHub o con Copilot. Deja que Copilot cree la PR, localice la incidencia pertinente y la relacione con ella.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Comprueba que **Agent** está seleccionado en el menú desplegable de agentes, para que Copilot pueda utilizar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot recopile la información y realice las operaciones. La primera vez que utilice una herramienta de GitHub, puede que te pida que te autentiques; sigue las instrucciones.
5. Observa cómo Copilot busca entre las incidencias, localiza la correcta y crea la PR.
6. Selecciona el enlace generado por Copilot para revisar la solicitud, pero **no la combines todavía**.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas disponibles para Copilot;
- has añadido el servidor MCP de Playwright al espacio de trabajo;
- lo has utilizado para probar manualmente la función en un navegador antes de publicarla;
- has publicado la rama y abierto una solicitud de incorporación de cambios.

Has utilizado un servidor MCP para *probar* una función, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
