---
title: "Ejercicio 2: Añadir funciones con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo habitual de desarrollo incluye localizar los archivos, hacer los cambios, ejecutar pruebas, depurar, averiguar qué archivo falta y volver a modificarlo. La lista continúa.

El modo de agente de Copilot puede ayudarte con este trabajo.

El modo de agente de Copilot funciona de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como pruebas y corrige automáticamente los errores que detecta. Veamos cómo utilizarlo para añadir funciones al sitio.

> [!NOTE]
> Aunque sus nombres son similares, el modo de agente y el agente en la nube ofrecen experiencias diferentes. El modo de agente trabaja en el IDE, permite ciclos rápidos de comentarios e interacción. El agente en la nube trabaja de forma asíncrona como un compañero del equipo y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo de agente de Copilot explora el proyecto, localiza archivos pertinentes y coordina cambios;
- el modo de agente de GitHub Copilot implementa funciones en la interfaz y la capa de datos;
- revisar los cambios y las pruebas generados antes de combinarlos con el código.

## Escenario

El catálogo de juegos está creciendo y quieres que los usuarios puedan filtrar por categoría y editorial. En el ejercicio anterior ya añadiste un helper de editoriales; ahora completarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecutar el sitio web Tailspin Toys

Antes de hacer cambios, explora el sitio web para conocer su funcionalidad actual.

Tailspin Toys es una plataforma de financiación colectiva para juegos de mesa con una temática de desarrollo. Permite consultar juegos y sus detalles. El sitio es una única aplicación Astro que genera HTML estático al compilar. Las páginas consultan directamente una base de datos SQLite local desde su frontmatter mediante Drizzle ORM: no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`; la interactividad se implementa con pequeños elementos `<script>` de Astro y las API estándar del DOM.

### Iniciar el sitio web

La tarea `npm run dev` inicia el único servidor de desarrollo Astro. Ejecútala en el codespace:

1. Vuelve al codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal nuevo en el codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y selecciona una rama nueva para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, aparecerá un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace de localhost en el terminal, se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Este túnel privado dirige al servidor web que ahora se ejecuta en el codespace.

### Explorar el sitio web

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Página de inicio**: muestra los juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo dependerán del ejercicio; cada paso indicará cuál debes utilizar.

La primera versión del sitio funciona, pero queremos añadir nuevas capacidades. Empieza revisando el backlog. Al crear el proyecto a partir de la plantilla, se creó automáticamente un backlog de incidencias; pide a GitHub Copilot que te lo muestre.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos de forma autónoma, ejecuta comandos de terminal e invoca herramientas.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no expresan preferencia por un modelo concreto. Durante la creación del taller se utilizó Claude Sonnet 4.5 y se incluye en estas instrucciones para que las sugerencias sean relativamente coherentes y la experiencia resulte adecuada. Sin embargo, los LLM son probabilísticos y las sugerencias pueden ser diferentes. Es normal y previsible.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede utilizar un comando MCP distinto, pero debería poder completar la tarea.

3. Envía este prompt para preguntar a Copilot por el backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot ha priorizado los elementos según lo que considera más útil para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo [de instrucciones de interfaz (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio web.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Lee las directrices generales para añadir funciones. Incluyen:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a instrucciones específicas para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Esto permite reutilizar instrucciones, dividir conjuntos complejos en partes más manejables y ofrecer ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar el filtrado, hay que hacer al menos tres cambios distintos en la aplicación:

- añadir o perfeccionar la lógica de filtrado restante en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo de agente de Copilot puede encargarse de estas tareas.

1. Continúa con la conversación actual de Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** siga seleccionado en el menú desplegable de agentes.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** siga seleccionado.
5. Pide a Copilot que implemente la función según la incidencia pertinente del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la función: los helpers de la capa de datos, la interfaz y las pruebas. A continuación, modifica los archivos y ejecuta las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos. Mientras tanto, pueden aparecer los botones **Keep** y **Undo** en el editor. Al terminar, tendrás la opción de conservar o deshacer cada cambio; no hace falta seleccionar nada mientras el agente sigue trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede haber pausas y algunas pruebas pueden fallar durante el proceso. Copilot alternará entre generar código y ejecutar pruebas hasta terminar la tarea y no detectar errores.

   ![Captura de una sesión completa de Copilot Agent Mode en Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se ejecuta el sitio y explora la nueva función.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a utilizar el modo de agente de GitHub Copilot para añadir funciones al sitio Tailspin Toys. Has visto cómo:

- el modo de agente de GitHub Copilot implementa funciones en la interfaz y la capa de datos;
- Copilot explora el proyecto, localiza archivos pertinentes y coordina cambios;
- revisar los cambios y las pruebas generados antes de combinarlos con el código.

Ahora [probarás la función con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Utiliza lo aprendido en este ejercicio para pedir a Copilot que actualice el sitio:

- sigue las prácticas recomendadas, incluidos los archivos de instrucciones existentes;
- decide cómo implementar la paginación, por ejemplo, si los usuarios podrán elegir el tamaño de página o si será fijo;
- proporciona a Copilot las directrices necesarias para generar la implementación que quieres;
- itera con GitHub Copilot y pídele cambios o aporta contexto adicional. Este es el proceso habitual al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
