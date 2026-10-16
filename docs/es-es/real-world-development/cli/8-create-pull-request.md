---
title: "Lección 8: Crear y combinar la PR de la función"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Ahora los revisarás en conjunto y abrirás una solicitud de incorporación de cambios. Combinaste tú mismo la solicitud de incorporación de cambios (PR) de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente, separaríamos la función, las actualizaciones de instrucciones, la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una misma conversación y rama, y enviarás todo ese trabajo en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación;
- inspeccionarás el cambio completo y las pruebas de verificación;
- crearás la PR del filtrado;
- habilitarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo del filtrado, has utilizado Copilot para planificar, implementar y verificar una función. Tailspin Toys quiere automatizar el resto del trabajo de la PR, manteniendo el control de la autorización para combinarla en manos del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para integrar una solicitud de incorporación de cambios. Al habilitarlo, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que han fallado, responde a los comentarios de revisión y vuelve a basar la rama cuando sea necesario. Después, habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora, has seleccionado tú mismo **Merge pull request**. Agent Merge puede encargarse de esa tarea. Revisa el trabajo y asegúrate de que está listo antes de habilitar Agent Merge.

## Usar Agent Merge para gestionar la PR

Ya has creado todo el código. Ahora lo revisarás, crearás la PR y dejarás que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente escribiendo `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo termine correctamente, la combinará.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni la falta de permisos. Si el proceso se bloquea, lee el motivo indicado y, cuando el repositorio lo permita, completa manualmente la combinación que ya has revisado.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y ahora gestionar la solicitud de incorporación de cambios. En concreto, has:

- aprendido qué es Agent Merge y cómo automatiza el ciclo de combinación;
- inspeccionado el cambio completo y las pruebas de verificación;
- creado la PR del filtrado;
- habilitado Agent Merge solo después de revisar los cambios y confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos slash útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos y las sesiones, y delegar trabajo opcionalmente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
