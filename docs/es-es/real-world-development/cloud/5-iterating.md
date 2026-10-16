---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este taller has colaborado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidad: documentar el código, crear una función de juegos relacionados para que el equipo de diseño pueda iterar y añadir funciones de accesibilidad, incluidos los selectores de modo claro y alto contraste. Revisemos los cambios y, si hace falta, pidamos a Copilot que los mejore.

### Escenario

Como hemos señalado, los fundamentos del diseño de software y DevOps no cambian al incorporar IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Revisarás las propuestas de Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y el agente de GitHub Copilot en la nube

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, se aplican restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para su trabajo.
- El agente se ejecuta en GitHub Actions, en un entorno independiente y efímero.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) generada por el agente: la que añade documentación al código. Utiliza la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Puede que veas un aviso de que un firewall ha bloqueado a GitHub Copilot. Es **normal**: el acceso del agente a los recursos externos está limitado de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para ver la lista de solicitudes.
3. Abre la PR titulada algo parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot aún está trabajando, la PR mostrará el indicador **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, incluidos los comentarios de documentación TSDoc y otros cambios. El resultado exacto puede variar. Busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios; proceden de los archivos de instrucciones personalizados que revisaste al principio del Ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la única aplicación Astro se superarán. El proceso puede tardar unos minutos.

## Pedir cambios a GitHub Copilot

La colaboración con Copilot no es unidireccional. Puedes mencionarlo en comentarios de la PR, como harías con otros miembros del equipo, o en comentarios insertados en el código. Copilot los tendrá en cuenta e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no hay una frase fija que debas utilizar. Puedes pedir, por ejemplo:

- que añada al principio de cada archivo de código un encabezado con una breve descripción;
- que añada comentarios TSDoc a los archivos TypeScript y Astro;
- que cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario solicitando un cambio en la documentación generada y menciona a **@copilot**. Puedes utilizar una de las ideas anteriores o proponer otra mejora.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para aplicar los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá una confirmación nueva en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla con la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la función de juegos relacionados

Vuelve a la PR que Copilot ha generado para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Ve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** y revisa el código generado.
5. Cuando hayas terminado, vuelve a **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, se superarán las comprobaciones del proyecto para la única aplicación Astro; el proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a la rama de Copilot en tu codespace y probar manualmente la función. Abre un terminal y ejecuta los comandos siguientes (sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes iterar y pedirle cambios, por ejemplo, ajustar el número de juegos o añadir encabezados y comentarios de documentación TSDoc (recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas). Añade un comentario en la pestaña **Conversation** y Copilot lo utilizará para iniciar otra sesión.

## Revisar las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. La PR debería incluir el modo de alto contraste que asignaste en el Ejercicio 3 y el modo claro que solicitaste desde mission control en el Ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR cuyo título se parezca a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR mostrará el indicador **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** y revisa el código generado.
5. Examina la implementación, en especial:
   - los selectores de la interfaz para cambiar de modo;
   - el almacenamiento local de las preferencias del usuario;
   - los cambios de CSS y estilo para los modos de alto contraste y claro;
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación mediante teclado;
   - el código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas terminado la revisión, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, se superarán las comprobaciones del proyecto para la única aplicación Astro; el proceso puede tardar unos minutos.
10. **Opcional:** cambia a la rama de Copilot en el codespace y prueba manualmente las funciones de accesibilidad. Abre un terminal y ejecuta lo siguiente (sustituye `<branch-name>` por el nombre de la rama creada por Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los selectores de modo claro y alto contraste en el navegador. Confirma que funcionan y que las preferencias se mantienen al recargar la página.

Observa cómo el agente personalizado de accesibilidad ha orientado a Copilot para implementar estas funciones según las prácticas recomendadas de accesibilidad. Si detectas algún problema o mejora posible, puedes mencionar a **@copilot** en un comentario para pedir cambios, como con las PR anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube es más útil cuando puedes asignarle trabajo real del backlog y ocuparte de otras tareas. Practica creando nuevas incidencias en el repositorio y asignándolas a Copilot. Algunas ideas:

- crear un formulario de interés para los patrocinadores en la página de detalles del juego;
- implementar la paginación de la lista de juegos;
- añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **has inspeccionado los archivos de instrucciones personalizados del repositorio** para observar su efecto posterior en el agente;
- **has asignado incidencias al agente de GitHub Copilot en la nube** y has visto cómo prepara el entorno, planifica y trabaja de forma asíncrona;
- **has creado y utilizado un agente personalizado** de accesibilidad para añadir selectores de modo claro y alto contraste;
- **has utilizado la página de agentes de Copilot como centro de control** para supervisar y orientar una sesión de accesibilidad;
- **has revisado e iterado sobre las solicitudes de incorporación de cambios del agente**, mencionado a `@copilot` para pedir cambios y aprobado flujos de trabajo.

## Repaso y siguientes pasos

Has terminado el recorrido del agente en la nube. Si quieres seguir aprendiendo, los demás recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración MCP directamente desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: publica dos cambios revisados desde el terminal mediante los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y la integración opcional `/delegate` con el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asignar al agente en la nube una incidencia de refactorización o cobertura de pruebas;
- crear un agente personalizado para otro ámbito;
- configurar `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Información sobre los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
