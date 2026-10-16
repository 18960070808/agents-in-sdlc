---
title: "Lección 5: Personalizar y utilizar la habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de sus informes y utilízala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Ya hemos comprobado manualmente que funciona y hemos utilizado archivos de instrucciones para asegurarnos de que sigue los estándares. Pero ¿qué ocurre con las pruebas, lint y las demás partes de la integración continua (CI)?

Para este tipo de tareas, las **habilidades de agente** son la opción adecuada. Ayudan a Copilot a ejecutar operaciones como estas de la forma correcta.

En esta lección:

- explorarás la habilidad `quality-checks` existente;
- personalizarás el formato de sus resultados;
- volverás a cargar y ejecutar la habilidad.

## Escenario

Tailspin Toys utiliza la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos de apoyo que un agente carga cuando los necesita. Una habilidad suele consistir en una carpeta con un archivo Markdown llamado `SKILL.md`. El archivo contiene frontmatter con un nombre y una descripción de la habilidad, una descripción general de lo que hace y directrices sobre cuándo debe invocarse. La carpeta también puede contener subcarpetas con scripts y otros recursos que utiliza la habilidad.

> [!NOTE]
> Una habilidad no necesita archivos ni carpetas adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, porque utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en `.github/skills` para compartirlas y reutilizarlas en el equipo o en la carpeta personal `~/.copilot/skills`.

## Explorar la habilidad

Explora la habilidad `quality-checks` que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee `name` y `description` al principio del archivo. La descripción ayuda a Copilot a decidir cuándo invocar la habilidad.
3. Lee las instrucciones y fíjate en cómo orientan a Copilot durante las pruebas y el proceso de lint.
4. Observa que la habilidad aún no tiene una sección **Results output formatting**.

## Ejecutar la habilidad antes de cambiarla

Puedes invocar habilidades directamente desde Copilot CLI o pedirlas en lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final.

## Personalizar el informe

Mejoremos el informe para que indique qué se ha ejecutado, si ha terminado correctamente y qué resultados han devuelto las herramientas. Actualiza la habilidad para generar este informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guardará automáticamente.

## Ejecutar la habilidad actualizada

Ahora puedes comprobar el resultado. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente el mismo prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- has explorado la habilidad `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos;
- has personalizado el formato de sus resultados;
- has vuelto a cargar y ejecutar la habilidad.

Ese cambio acompañará al filtrado en la PR de la función. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas en el navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
