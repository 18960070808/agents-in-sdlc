---
title: "Ejercicio 1: Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de GitHub Copilot en la nube →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta, o Copilot debería conocer información de fondo, conviene asegurarse de que ese contexto esté disponible. Los [archivos de instrucciones][instruction-files] proporcionan esas directrices para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* debe hacerlo.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones con ámbito de ruta;
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás el resultado de un prompt antes y después de cambiar las instrucciones. El agente de GitHub Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar ese efecto en tiempo real. Verás la influencia del archivo de instrucciones más adelante, al revisar las solicitudes de incorporación de cambios que genere el agente.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys cuenta con directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe estar en modo oscuro y tener un aspecto moderno.
- El código debe documentarse con comentarios de documentación TSDoc.
- Al principio de cada archivo debe añadirse un bloque de comentarios que explique su función.

Con los archivos de instrucciones, te asegurarás de que Copilot tenga la información necesaria para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas permiten proporcionar contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Puedes indicar tus convenciones de programación, bibliotecas y tipos de comentarios preferidos. También puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto relevante para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción del proyecto, las prácticas recomendadas y otras directrices generales.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> En un IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las finalizaciones de código ni para las sugerencias de la siguiente edición.
>
> Copilot Chat, Copilot CLI y Copilot cloud agent utilizan los archivos de instrucciones del repositorio y los archivos `*.instructions.md` con frontmatter `applyTo` al generar código.
>
> Copilot también [admite archivos de instrucciones que utilizan otros estándares][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar los archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en las directrices del proyecto: qué se está desarrollando, cómo se organiza el proyecto y cuáles son los estándares generales de programación.
- Utiliza archivos `*.instructions.md` para dar indicaciones específicas según el tipo de archivo (pruebas unitarias, componentes Astro o capa de datos) o la tarea.
- Utiliza lenguaje natural y directrices claras. Incluye ejemplos de cómo debería ser el código y de cómo no debería ser.

No hay una única forma de crear archivos de instrucciones, del mismo modo que no hay una única forma de utilizar la IA. La experimentación te permitirá averiguar qué funciona mejor en tu proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener un conjunto sólido de archivos de instrucciones. En este proyecto hay archivos para muchos tipos de tareas, incluidas las [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); la lección correspondiente lo indicará cuando sea necesario.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas de este proyecto

Lee los archivos de instrucciones que incluye este repositorio: hay un `copilot-instructions.md` principal y varios archivos `*.instructions.md` para distintas tareas. Puedes abrirlos en la interfaz web de GitHub o en el editor.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción breve del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas **GitHub Actions Workflows**, que se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior establece un patrón glob relativo a la raíz del repositorio. En este caso, coincidirá cualquier archivo de pruebas TypeScript, por ejemplo uno que siga el patrón `**/*.test.ts`.
5. Fíjate en las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos del proyecto. Esto permite dividir conjuntos de instrucciones grandes en archivos reutilizables y mostrar a Copilot ejemplos que puede seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación, pero aún no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios de documentación TSDoc y los encabezados de comentarios de archivo.

## Añadir un nuevo estándar del repositorio

Solo editarás una parte en este ejercicio: añadirás una regla general para que la documentación se incluya en el código como comentarios TSDoc y encabezados a nivel de archivo. El agente en la nube utilizará esta regla en las incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente en la nube lee las instrucciones de la rama de destino de la incidencia, pero de todos modos confirmarás los cambios:

1. Desde el terminal del codespace, crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Documenta los estándares del repositorio para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que aún no incluye reglas de documentación en el código.
3. Justo debajo de los estándares existentes, añade estas líneas Markdown para pedir a Copilot que incluya encabezados de comentario de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, puedes crear archivos de instrucciones en el ámbito del repositorio (`.github/copilot-instructions.md`) para establecer directrices generales o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios de documentación que acabas de añadir.

## Confirmar, enviar y combinar la actualización de instrucciones

El agente en la nube lee las instrucciones de la rama a la que se dirige la incidencia. Cuando le asignes una incidencia en el próximo ejercicio, Copilot partirá de `main`; por tanto, las instrucciones nuevas deben llegar a `main` para que el agente pueda utilizarlas.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y combínala. Así, el agente en la nube leerá las instrucciones al trabajar en las incidencias que le asignes en los siguientes ejercicios.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante el taller, puedes omitir la rama y confirmar el cambio directamente en `main`. Se utiliza una rama para reflejar el proceso habitual de un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y has añadido un nuevo estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica cuando el agente en la nube genere solicitudes de incorporación de cambios en las lecciones siguientes.

A continuación, [asignarás tu primera incidencia al agente de GitHub Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
