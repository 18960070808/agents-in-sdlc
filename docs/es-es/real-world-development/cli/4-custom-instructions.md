---
title: "Lección 4: Orientar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta, conviene que Copilot disponga de esas indicaciones. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has desarrollado el filtrado, explorarás las instrucciones que ha utilizado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta;
- actualizarás el archivo de instrucciones para garantizar que se siguen los estándares de programación;
- comprobarás el efecto de los archivos de instrucciones en el código.

## Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys cuenta con directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones que no sean evidentes, no limitarse a repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, sus parámetros y sus valores devueltos mediante TSDoc/JSDoc, incluido el argumento `db` inyectable cuando corresponda.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben conservar las directrices existentes de formato y lint.

Con los archivos de instrucciones, te asegurarás de que Copilot tenga la información necesaria para llevar a cabo las tareas de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas permiten proporcionar contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función puede ayudarte a obtener sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de programación, las bibliotecas y hasta los tipos de comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos, a fin de proporcionar contexto para una tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo de instrucciones que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto relevante para la mayoría de las solicitudes que envías a Copilot desde el chat o la CLI.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explorar los archivos de instrucciones personalizadas de este proyecto

El proyecto de inicio ya incluye varios archivos de instrucciones. Antes de hacer cambios, explóralos para ver qué contienen.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la breve descripción del proyecto y en sus directrices de programación. Se aplican a todas las interacciones de Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, las pruebas y otros temas.
5. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior establece un patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que puede seguir.

## Actualizar las instrucciones para que reflejen las directrices del equipo

Los archivos existentes son un buen comienzo, pero aún falta algo. Modifica el archivo principal `copilot-instructions.md` para que el código TypeScript nuevo incluya comentarios TSDoc.

1. En `.github/copilot-instructions.md`, localiza la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade esta viñeta al final:

   ```markdown
   - Todo el código TypeScript nuevo debe incluir comentarios TSDoc con fines de documentación.
   ```

El archivo se guardará automáticamente.

## Usar las directrices actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar los cambios, sal de la conversación de filtrado y retómala para que Copilot cargue las instrucciones nuevas sin perder el contexto de la función.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, retoma la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está activa la rama `game-filters-cli`.
3. Pide a Copilot que aplique las instrucciones actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Fíjate en los comentarios TSDoc nuevos y confirma que explican el código correctamente.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI obtiene contexto de los archivos de instrucciones del proyecto y has aplicado un nuevo estándar a la función. En concreto, has:

- explorado cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta;
- actualizado el archivo de instrucciones para que se sigan los estándares de programación;
- observado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para garantizar que se ejecuten las pruebas y lint de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
