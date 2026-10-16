---
title: "Ejercicio 2: Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que haya pocas organizaciones que no tengan problemas con la deuda técnica. Puede tratarse de incidencias de seguridad pendientes, código heredado que debe actualizarse o funciones que llevan tiempo en el backlog porque nunca ha habido tiempo para implementarlas. El agente de GitHub Copilot en la nube puede actualizar código y añadir funciones de forma autónoma. Cuando termina, crea una solicitud de incorporación de cambios en borrador para que la revise un desarrollador. Así se pueden delegar tareas tediosas, acelerar el desarrollo y dejar que los desarrolladores se centren en trabajos de mayor alcance.

En este ejercicio explorarás cómo utilizar el agente en la nube para:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realicen de forma segura;
- comprender la importancia de delimitar claramente las incidencias;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado inadecuado: comprobarás que no hay ninguna. Como primer paso, el equipo quiere añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere facilitar el descubrimiento de juegos. En la página de detalles de cada juego quiere mostrar otros títulos de la misma categoría para que los visitantes puedan seguir explorando. Por ahora no hace falta una implementación pulida; quieren probar la experiencia de usuario y aceptar el diseño. Esta tarea está bloqueada, aunque hay otras incidencias de mayor prioridad.

Estas tareas suelen posponerse y son buenas candidatas para el agente en la nube. Este puede trabajar en segundo plano mientras el desarrollador se ocupa de otras tareas, y después revisar el trabajo propuesto.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se la asignas, Copilot crea una solicitud de incorporación de cambios en borrador para registrar el progreso, prepara el entorno y empieza a trabajar. Puedes consultar su sesión mientras trabaja o cuando haya terminado. Cuando la solución propuesta esté lista para revisarla, te mencionará en la PR.

## Importancia de unas instrucciones bien delimitadas

GitHub Copilot no hace magia ni elimina la necesidad de pensar. Incluso las operaciones que parecen sencillas pueden volverse complejas cuando se examinan con detalle. Ten cuidado al delimitar las tareas para el agente en la nube y sigue las [prácticas recomendadas][cloud-agent-best-practices]. Trátalo como un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas orientan a Copilot. Si ya has completado el ejercicio práctico sobre instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`; si no, puedes revisarlo ahora. Antes de asignar trabajo al agente, examina los archivos de instrucciones del repositorio sin modificarlos.

Abre estos archivos en la interfaz web de GitHub o en un codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que limita estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` al agente en la nube, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentarios del código generado. Proceden de estos archivos de instrucciones. Volveremos a señalarlos al revisar las PR en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente en la nube

El desarrollo de código suele requerir un entorno concreto y la ejecución de scripts de configuración para disponer de las herramientas y dependencias necesarias. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] para preparar su entorno. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. Así puede acceder a las herramientas y dependencias de desarrollo necesarias. La plantilla lo configura de antemano para el taller: instala Node.js y las dependencias del proyecto, los navegadores para las pruebas de extremo a extremo y prepara la base de datos SQLite migrada y con datos iniciales para la aplicación Astro.

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Concede solo los permisos mínimos que necesiten estos pasos. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo para instalar dependencias, necesitan el permiso contents: read. Si no se clona el repositorio, Copilot lo hará automáticamente cuando terminen los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (la aplicación completa ahora utiliza Astro y Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y añade datos a la base de datos SQLite local para que la compilación y las pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero hay dos aspectos que conviene destacar:

- Contiene un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El activador `workflow_dispatch` permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Esto sirve para comprobar que funciona sin tener que esperar a que Copilot lo ejecute.

## Añadir documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos contienen información obsoleta o carecen de ella. Esta deuda técnica reduce la productividad y dificulta mantener el código o incorporar a nuevos desarrolladores. Copilot puede generar documentación, por lo que esta es una incidencia adecuada para el agente en la nube. En un ejercicio posterior revisarás su trabajo.

1. Abre una pestaña nueva del navegador y ve al repositorio en github.com.
2. Selecciona **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe el texto siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

   ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) generada, que tendrá un título parecido a `[WIP]: Code lacks documentation`. Si no aparece ninguna PR, espera un momento y actualiza el navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno mediante el flujo de trabajo anterior, analizando el proyecto y decidiendo cómo abordar la tarea.

13. Revisa la lista de tareas que completará.
14. Desplázate por la cronología de la PR hasta encontrar una actualización que indique que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

   ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Desplázate por la sesión en directo y observa cómo Copilot resuelve la tarea: explora el código y su estado, se detiene para analizar y decidir un plan, y después crea código.

El proceso tardará unos minutos. Una de las principales ventajas del agente en la nube es que puede trabajar de forma asíncrona mientras tú te ocupas de otras tareas. Aprovecharemos esta función y le asignaremos otra tarea; después, nos centraremos en desarrollar funciones para la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

El agente de GitHub Copilot en la nube permite dividir el trabajo: puedes ocuparte de unas tareas mientras Copilot se centra en otras. Añadir una sección de juegos relacionados quizá no tarde mucho, pero puedes aprovechar ese tiempo para avanzar en otros asuntos. Vamos a asignar la tarea al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe el texto siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create**.
8. A la derecha, selecciona **Assign to Copilot**.

   ![Asignar una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, aparecerá un 👀 en el primer comentario de la incidencia, indicando que Copilot ha empezado a trabajar.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda tarea. El agente en la nube funciona de forma parecida a un ingeniero de software: no hace falta supervisarlo constantemente, basta con revisar el resultado cuando termine. Mientras tanto, aprenderás a crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona en deuda técnica, funciones o migraciones entre frameworks.

Has explorado:

- la personalización del entorno de generación de código;
- cómo garantizar que las operaciones se realicen de forma segura;
- la importancia de delimitar claramente las incidencias;
- cómo asignar incidencias a Copilot.

Mientras el agente trabaja en segundo plano, aprenderás a crear y utilizar agentes personalizados. [El agente en la nube también puede usar servidores MCP][cloud-agent-mcp] y tiene acceso a las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Información sobre el agente de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
