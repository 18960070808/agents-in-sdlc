---
title: "Lección 3: Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para desarrollar el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has empezado añadiendo una pequeña función al proyecto, pero los cambios más amplios requieren un proceso más sólido. GitHub Copilot CLI está diseñado para integrarse en el flujo de trabajo existente de una organización y ayudarte a desarrollar lo adecuado de la forma correcta. Esta es la primera de varias lecciones en las que seguirás un proceso de desarrollo habitual con agentes: partirás de una incidencia para crear una función, comprobarás que el código es válido y que la función se comporta como se espera y, finalmente, la integrarás en el proyecto.

> [!NOTE]
> Seguirás trabajando en la misma conversación y rama durante todo el flujo de trabajo de esta función. Normalmente utilizarías ramas o PR distintas para los diferentes tipos de archivos, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación nueva de Copilot a partir de una incidencia de GitHub;
- definirás los requisitos en modo Plan;
- implementarás la función en modo Autopilot;
- revisarás el código;
- validarás manualmente la función en el navegador a través del puerto reenviado de Codespaces.

Para continuar con esta función, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás validación MCP, crearás un agente de QA y abrirás la PR de la función.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la función, pero antes de programar hay que acordar detalles como la combinación de categorías. Utilizarás el modo Plan para tomar esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

La incorporación de agentes de programación con IA al flujo de desarrollo no cambia los fundamentos. De hecho, hace que sean aún más importantes. La mayoría de los desarrolladores sigue un proceso parecido a este:

1. Empezar con una incidencia registrada que describa el trabajo necesario.
2. Crear un plan con lo que hay que desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Una vez revisado el código y superada la integración continua, combinarlo.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos se parecen al descrito.

Seguir este enfoque estándar permite comprobar que el código generado por IA cumple los requisitos y pasa por el mismo proceso de validación que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** determina el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: tú y el agente trabajáis juntos. El agente sugiere cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta que considera que la tarea está terminada.

Empieza en modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Partir de la incidencia

Antes de empezar el trabajo de filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo.
2. Confirma que se ha combinado la PR de valoración con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt empieza ahora por `#7` (o un número parecido). El prefijo `#` permite incluir en el contexto una incidencia o solicitud de incorporación de cambios (PR) de GitHub.

## Planificar la función de filtrado

La planificación te permite definir el enfoque de una función o tarea antes de delegarla a Copilot. Siempre es recomendable dedicar algo de tiempo a planificar las tareas complejas. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador situado debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, escribe este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y determinará el mejor enfoque para desarrollar la función.

3. Durante el proceso, Copilot puede hacerte preguntas sobre el comportamiento del filtrado. Respóndelas según tus preferencias; no hay una única respuesta correcta.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que modifique cualquier parte que no refleje tus decisiones.

## Aprobar Autopilot

Una vez redactado y revisado el plan, puedes implementarlo con Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considere que la tarea está terminada.

1. Selecciona **Accept plan and build on autopilot (recommended)** o una opción con un nombre parecido en tu versión.
2. Confirma que el indicador situado debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente, así que revísalo antes. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisar y verificar la implementación

Una vez generado el código, hay que revisarlo antes de combinarlo, como cualquier otro cambio. Revisa el código y ejecuta el sitio para comprobar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones que has tomado durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija cualquier error.

## Explorar la nueva función

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio en el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría, por editorial y las combinaciones acordadas en el plan.
4. Confirma que el comportamiento al restablecer los filtros y cuando no hay resultados coincide con la incidencia y con tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y siguientes pasos

Has utilizado diferentes modos de conversación para desarrollar y revisar una función. En esta lección:

- has iniciado una conversación nueva de Copilot a partir de una incidencia de GitHub;
- has definido los requisitos en modo Plan;
- has implementado la función en modo Autopilot;
- has revisado el código;
- has validado manualmente la función en el navegador a través del puerto reenviado de Codespaces.

A continuación, profundizarás en la generación de código y te asegurarás de que siga las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
