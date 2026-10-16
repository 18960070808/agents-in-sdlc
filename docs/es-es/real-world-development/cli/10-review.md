---
title: "Lección 10: Repaso y pasos siguientes"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y más recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI durante un flujo de trabajo continuo en Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y localizado la incidencia de filtrado inicial;
- añadido valoraciones con estrellas, revisado el resultado en el navegador a través del puerto reenviado de Codespaces y combinado manualmente tu primera solicitud de incorporación de cambios (PR);
- partido de la incidencia de filtrado, definido el enfoque en modo Plan, desarrollado en modo Autopilot y revisado en modo Interactive;
- orientado al agente con instrucciones personalizadas, adaptado la habilidad `quality-checks` existente y utilizado esta para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- añadido el servidor Model Context Protocol (MCP) de Playwright y utilizado para explorar el filtrado en un navegador real;
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas del navegador;
- revisado el cambio completo de filtrado y autorizado Agent Merge para la PR de filtrado;
- explorado comandos slash para consultar el contexto y los modelos, compartir sesiones y delegar trabajo opcionalmente en la nube.

## Qué has publicado

El taller tiene dos hitos de PR, cada uno en su propia rama basada en `main` actualizado:

1. **Valoraciones con estrellas:** muestra `starRating` y un estado explícito de falta de valoración en las tarjetas de juegos.
2. **Filtrado y flujo de trabajo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la función, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, has utilizado la misma conversación y rama. Hemos combinado ese trabajo en una sola PR para simplificar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, inspeccionándolo en el navegador y dejando que Copilot lo explorara mediante MCP. La habilidad `quality-checks` ha ejecutado pruebas unitarias, lint y comprobaciones de tipos, y ha presentado los resultados en el nuevo formato. QA ha reunido esos resultados con una revisión de los requisitos y la cobertura de pruebas antes de la PR.

Las pruebas añadidas deben cubrir carencias reales; una ejecución de QA que no requiera pruebas nuevas también puede ser correcta. Las herramientas que falten, las comprobaciones omitidas y los errores son bloqueos visibles, no resultados satisfactorios. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las pruebas afectadas después de cualquier cambio.

## Buenas prácticas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen funciones especializadas y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados reales de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** según la tarea. Usa **Plan** para analizar el enfoque antes de empezar, **Interactive** para intervenir en cambios concretos y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para ediciones rutinarias y otro más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres desarrollar, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más posibilidades

Ya has recorrido el flujo de trabajo principal. Estas son algunas funciones más de CLI que puedes explorar:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para analizar un problema y obtener otra perspectiva;
- `/fleet` para coordinar tareas independientes en paralelo;
- `/worktree` para aislar otra tarea;
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Utilízala para código de producción, proyectos personales o esa pequeña aplicación que llevas años pensando en crear. Comparte lo que has aprendido con el equipo y aprende de los demás. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Información sobre GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
