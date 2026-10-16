---
title: "Ejercicio 3: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos del flujo de desarrollo. Al definir agentes en archivos Markdown de `.github/agents` en el repositorio, puedes proporcionar instrucciones específicas, prácticas recomendadas, patrones de código y conocimientos del ámbito. Así, Copilot puede llevar a cabo determinados tipos de trabajo con mayor eficacia. Los equipos pueden codificar sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que compruebe el cumplimiento de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen mediante archivos Markdown en `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, como mínimo, `name` y `description`, seguido de un prompt Markdown que define el comportamiento, los conocimientos y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen aspectos en común: ambos se definen principalmente mediante archivos Markdown y explican a la IA cómo realizar operaciones. La diferencia más clara es que el **agente personalizado** realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y coordinan habilidades e incluso otros agentes. En este taller, el agente de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad; puede utilizar habilidades, por ejemplo, para gestionar un flujo de trabajo de solicitud de incorporación de cambios o ejecutar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como con cualquier función de IA, prueba e itera para averiguar qué funciona mejor en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Explorarás los siguientes aspectos de los agentes personalizados:

- cómo se definen;
- cómo asignarles una tarea.

## Escenario

Tailspin Toys se compromete a que su plataforma de financiación colectiva sea accesible para todos, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes indican que a algunos usuarios les cuesta leer el tema oscuro actual debido al escaso contraste entre el texto y el fondo. Para resolverlo, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, así que quieres implementar la función cuanto antes. Utilizarás un agente personalizado para generarla.

## Revisar el agente de accesibilidad personalizado

Ya se ha creado un agente de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve a tu codespace y revisa el archivo del agente:

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Lee las secciones siguientes, que destacan:
   - responsabilidades principales al generar código para un sitio accesible;
   - prácticas recomendadas de accesibilidad;
   - ejemplos de código para HTML, CSS y JavaScript;
   - errores habituales.

## Crear y asignar una incidencia

Mission control es el centro desde el que puedes trabajar con los agentes del entorno. Permite asignar tareas al agente de Copilot en la nube, supervisarlas y redirigirlas u ofrecer instrucciones adicionales. Asígnale una tarea para crear el modo de alto contraste.

1. Ve al repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Add high contrast mode to website` en **Title**.
6. Introduce el texto siguiente en **Description**:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.
9. En la lista de agentes personalizados, selecciona **Accessibility agent**.

    ![Captura de la asignación del agente en la nube con el agente personalizado Accessibility seleccionado](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados en tareas y ámbitos concretos. Permiten codificar los conocimientos y los estándares del equipo en agentes reutilizables que guían el trabajo de Copilot.

Has explorado:

- cómo se definen los agentes personalizados;
- cómo asignarles una tarea.

Mientras Copilot trabaja en el modo de alto contraste, [supervisarás y orientarás la sesión del agente][next-lesson] desde mission control.

## Recursos

- [Información sobre agentes personalizados][custom-agents]
- [Preparar la organización para utilizar agentes personalizados][org-custom-agents]
- [Preparar la empresa para utilizar agentes personalizados][enterprise-custom-agents]

---

| [← Lección anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
