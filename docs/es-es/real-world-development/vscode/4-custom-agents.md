---
title: "Ejercicio 4: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos del flujo de desarrollo. Al definir agentes en archivos Markdown de la carpeta `.github/agents` del repositorio, puedes proporcionar instrucciones específicas, prácticas recomendadas, patrones de código y conocimientos del ámbito para que Copilot lleve a cabo determinados tipos de trabajo con mayor eficacia. Los equipos pueden codificar sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que compruebe el cumplimiento de [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen mediante archivos Markdown en `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, como mínimo, los campos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, los conocimientos y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunos aspectos en común: ambos se definen principalmente mediante archivos Markdown y explican a la IA cómo realizar operaciones. La diferencia más clara es que el **agente personalizado** realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes. En este taller, el agente personalizado de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad; puede utilizar habilidades como la de flujos de trabajo de solicitudes de incorporación de cambios o la que ejecuta y gestiona pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como con cualquier función de IA, prueba e itera para encontrar lo que mejor funciona en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que su plataforma de financiación colectiva sea accesible para todos, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes indican que a algunos usuarios les cuesta leer el tema oscuro actual porque el contraste entre el texto y el fondo es insuficiente. Para resolver este problema, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es importante, así que quieres implementar la función cuanto antes. Utilizarás un agente personalizado para generar la funcionalidad.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente;
- utilizarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve a tu codespace y cambia a una rama nueva basada en `main` para el trabajo de accesibilidad (mantendrás separada la PR de filtrado del ejercicio 3):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Lee las secciones siguientes, que destacan:
   - responsabilidades principales al generar código para un sitio accesible;
   - prácticas recomendadas de accesibilidad;
   - ejemplos de código para HTML, CSS y JavaScript;
   - errores habituales.

## Usar el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el menú desplegable de agentes, en la parte inferior de la vista Copilot Chat. Puedes seleccionar uno para limitar la sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo dependerán del ejercicio; cada paso indicará cuál debes utilizar.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** si aún no está seleccionado.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el menú desplegable de agentes situado en la parte inferior de la vista Chat; muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía este prompt al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código, propondrá cambios y los aplicará a los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y un diff que podrás revisar.
6. El indicador **Files changed** se actualizará a medida que el agente modifique los archivos.

> [!NOTE]
> El proceso puede tardar unos minutos. Copilot está modificando el repositorio: editará archivos existentes, como los componentes Astro, los estilos y las pruebas relacionadas.

En el siguiente ejercicio revisarás y orientarás el trabajo que está en curso.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados en tareas y ámbitos concretos. Permiten codificar los conocimientos y los estándares del equipo en agentes reutilizables que guían el trabajo de Copilot.

Has explorado:

- cómo se definen los agentes personalizados;
- cómo utilizar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson]: revisarás los cambios a medida que se realicen y añadirás un selector de modo claro a la misma sesión.

## Recursos

- [Información sobre agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la función con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
