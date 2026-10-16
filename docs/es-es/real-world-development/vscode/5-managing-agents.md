---
title: "Ejercicio 5: Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando pones a GitHub Copilot en modo de agente, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como trabaja en el equipo en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición. Puedes revisar los diffs en línea, aceptar o rechazar cambios individuales y orientar la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás las ediciones propuestas;
- orientarás al agente durante la sesión para añadir un selector de modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha solicitado además un selector de modo claro para quienes consideran que el tema predeterminado es demasiado oscuro. En lugar de iniciar una conversación nueva, orientarás la sesión actual para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con un diff y controles **Keep**/**Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para seguir las llamadas a herramientas, lecturas y ediciones de archivos y los comandos de terminal que solicite ejecutar.
3. Cuando proponga una edición, el diff aparecerá en línea:
   - selecciona **Keep** para aceptar el cambio;
   - selecciona **Undo** para deshacerlo y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed**, situado en la parte superior del chat, para ver todos los archivos que el agente ha modificado durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará el aviso **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final que resuma los cambios.

## Pedir al agente que añada el modo claro

Cuando esté implementado el modo de alto contraste, amplía la misma conversación y añade un selector de modo claro. El agente conserva el contexto de su trabajo anterior: los componentes modificados, el enfoque de estilos y el patrón de almacenamiento local que ha establecido.

1. Envía este prompt de seguimiento desde Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará a partir del trabajo anterior, explorará los cambios existentes y propondrá cómo añadir el modo claro.
3. Revisa cada nueva edición a medida que aparezca y selecciona **Keep** o **Undo** en cada diff.
4. Si el agente toma una dirección que no quieres (por ejemplo, intenta reorganizar la interfaz de los selectores de una forma que no te gusta), detenlo con el botón **Stop** situado junto al cuadro de texto y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de cero. Envía prompts de seguimiento para pedir ajustes («simplifica el CSS: utiliza variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer selector; integra el modo claro en el panel de configuración actual») o ampliar el alcance («añade también métodos abreviados de teclado para ambos selectores»).

## Revisar los cambios

Antes de confirmar los cambios, revisa todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías encontrar cambios en componentes Astro, estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Confirma que se siguen los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación mediante teclado, HTML semántico y persistencia con almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo de agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has explorado:

- la supervisión del trabajo del agente en tiempo real;
- la aceptación y el rechazo de ediciones individuales;
- la orientación de una sesión en curso mediante prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios generados por el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
