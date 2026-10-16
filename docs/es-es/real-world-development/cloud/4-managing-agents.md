---
title: "Ejercicio 4: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente de GitHub Copilot en la nube que realice tres tareas distintas para mejorar la experiencia de usuario y añadir funciones. Aunque el agente trabaja de forma autónoma y asíncrona, también es importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas con solicitudes de incorporación de cambios abiertas, consultar las operaciones realizadas y orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- orientarás una sesión en curso para solicitar nuevas funciones.

## Escenario

Después de asignar al agente la implementación del modo de alto contraste, el equipo ha decidido añadir también un modo claro. Como ya se estaban actualizando los estilos del sitio y añadiendo selectores, parece lógico incluir esta función. Orientarás el trabajo del agente para que implemente tanto el modo claro como el alto contraste.

## Revisar las tareas del agente de GitHub Copilot en la nube

Consulta el estado actual de todas las tareas asignadas al agente en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel de la izquierda. Deberían aparecer las que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las operaciones que ha realizado Copilot. Pueden incluir:
   - clonar el código del repositorio;
   - crear el entorno de trabajo;
   - configurar servidores MCP;
   - realizar los pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos que aparezcan dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Fíjate también en el panel de solicitudes de incorporación de cambios de la derecha, donde podrás consultar la PR y los archivos modificados.

## Orientar al agente en la nube

Ahora que has consultado las tareas activas, pide a Copilot que añada un selector de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos hasta que haya terminado la configuración y empiece a trabajar. Lo sabrás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la página de agentes con el cuadro de diálogo para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora a su flujo de trabajo.

## Dejar que Copilot trabaje

Copilot continuará con la tarea actualizada e incorporará la petición nueva después de completar el paso en curso. El proceso tardará unos minutos; puedes aprovecharlos para repasar lo aprendido.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir sesiones en curso.

Has explorado:

- la supervisión de las tareas del agente desde mission control y la página de agentes;
- la modificación de una sesión en curso para solicitar una función adicional.

Mientras Copilot termina el trabajo sobre las funciones de accesibilidad, [revisarás e iterarás sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Información sobre agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
