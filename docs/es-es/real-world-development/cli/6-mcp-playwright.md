---
title: "Lección 6: Validar funciones con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar la función de filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos señalado, programar no consiste únicamente en escribir código. Hay que trabajar con datos y servicios externos, e incluso permitir que Copilot utilice automatizaciones adicionales. Para eso sirven los servidores MCP: permiten que Copilot amplíe las funciones integradas en la CLI y utilice más herramientas y servicios.

En esta lección:

- aprenderás qué es el Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI;
- añadirás el servidor MCP de Playwright si aún no está disponible;
- pedirás al agente que controle un navegador y explore la función de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz hay que interactuar con ella. Quieres que Copilot pueda utilizar el sitio como lo haría un usuario y automatizar así una parte mayor de la verificación de los cambios.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden consultar información actualizada y realizar acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos mediante un servidor MCP, que actúa como puente entre el agente y las herramientas o servicios externos. Cada servidor MCP ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server][github-mcp]**: permite acceder a las API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: ofrece funciones de automatización del navegador con Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa su código fuente, verifica quién lo publica y evalúa las implicaciones de seguridad.

## Añadir el servidor MCP de Playwright

Si el servidor MCP de Playwright todavía no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría un usuario.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la siguiente sección.

1. Vuelve a tu Codespace.
2. En Copilot CLI, escribe el siguiente comando para abrir el cuadro de diálogo para añadir un servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma que el tipo de servidor es **STDIO** pulsando <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pedir a Copilot que explore la función con Playwright

Ya has comprobado manualmente que la función se comporta como se esperaba. Ahora utiliza Playwright MCP para hacer lo mismo.

1. Envía este prompt para pedir a Copilot que utilice el servidor MCP de Playwright y valide la función:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No es necesario pedir explícitamente a Copilot que utilice el servidor MCP; normalmente lo detectará por sí mismo. Sin embargo, indicarle qué herramienta utilizar puede hacer que los resultados sean más coherentes y ahorrar algunos tokens.

2. Observa los pasos que Copilot realiza en el navegador para comprobar que la función funciona.
3. Lee el informe y confirma que todo se comporta como se esperaba.

Copilot iniciará el servidor, utilizará Playwright para interactuar con el sitio, detendrá el servidor y te proporcionará un informe.

## Resumen y siguientes pasos

¡Enhorabuena! Has utilizado el servidor MCP de Playwright para explorar la función en un navegador real desde Copilot CLI. En resumen, has:

- aprendido qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI;
- añadido el servidor MCP de Playwright si aún no estaba disponible;
- pedido al agente que controlara un navegador y explorara la función de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combine la habilidad y las herramientas del navegador en una función especializada.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
