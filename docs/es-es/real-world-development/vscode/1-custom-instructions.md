---
title: "Ejercicio 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta, o Copilot debería conocer información de fondo, conviene asegurarse de que ese contexto esté disponible. Hay varias formas de compartir contexto específico con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que proporcionan directrices sobre la generación de código.

En este ejercicio:

- explorarás cómo llega a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones con ámbito de ruta;
- generarás el primer bloque de datos para el filtrado (un helper de editoriales) con las instrucciones *actuales*;
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`;
- repetirás el mismo prompt y observarás cómo el código generado adopta el nuevo estándar;
- confirmarás los cambios de instrucciones y el bloque de filtrado en `main` para que Copilot pueda utilizarlos en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede no seguir algunos de los estándares que definas. Copilot no es determinista. El objetivo de este ejercicio es observar la *tendencia* del cambio de comportamiento tras actualizar las instrucciones, no conseguir una salida idéntica carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe estar en modo oscuro y tener un aspecto moderno.
- El código debe documentarse con comentarios de documentación TSDoc.
- Al principio de cada archivo debe añadirse un bloque de comentarios que explique su función.

Con los archivos de instrucciones, te asegurarás de que Copilot tenga la información necesaria para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas permiten proporcionar contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función puede ayudarte a obtener sugerencias y fragmentos de código más pertinentes. Puedes especificar las convenciones de programación, las bibliotecas y hasta los tipos de comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos, a fin de proporcionar contexto para una tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto relevante para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción de lo que se está creando, las prácticas recomendadas y otras directrices generales.
- Los archivos `.github/instructions/*.instructions.md` proporcionan directrices para lenguajes, tipos de archivo o tareas concretas; por ejemplo, TypeScript, Astro, componentes de interfaz o nuevas pruebas unitarias.

> [!NOTE]
> En un IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para las finalizaciones de código ni para las sugerencias de la siguiente edición.
>
> Copilot Chat, Copilot CLI y Copilot cloud agent utilizan los archivos de instrucciones del repositorio y los archivos `*.instructions.md` que tengan frontmatter `applyTo`.
>
> Además, Copilot [admite archivos de instrucciones que utilizan otros estándares][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar los archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque representativo para crear archivos de instrucciones:

- Mantén `copilot-instructions.md` centrado en las directrices del proyecto: qué se está desarrollando, cómo se organiza el proyecto y cuáles son los estándares generales de programación.
- Utiliza archivos `*.instructions.md` para dar indicaciones específicas según el tipo de archivo (pruebas unitarias, componentes Astro o capa de datos) o la tarea.
- Utiliza lenguaje natural y directrices claras. Incluye ejemplos de cómo debería ser el código y de cómo no debería ser.

No hay una única forma de crear archivos de instrucciones, del mismo modo que no hay una única forma de utilizar la IA. La experimentación te permitirá averiguar qué funciona mejor en tu proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener un conjunto sólido de archivos de instrucciones. Al explorar los de este proyecto, verás instrucciones para muchos tipos de tareas, como [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); la lección correspondiente lo indicará cuando sea necesario.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un `copilot-instructions.md` principal y varios archivos `*.instructions.md` para diferentes tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción breve del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior establece un patrón glob, relativo a la raíz del repositorio, que determina a qué archivos se aplican las instrucciones. En este caso, coincidirá cualquier archivo de pruebas TypeScript, como los que siguen el patrón `**/*.test.ts`.
5. Fíjate en las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos del proyecto. Esto permite dividir conjuntos de instrucciones grandes en archivos reutilizables y mostrar a Copilot ejemplos que puede seguir al generar código. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios de documentación TSDoc y los encabezados de comentarios de archivo.

## Crear una rama para los cambios

Sigue las buenas prácticas y crea una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo en el codespace pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los siguientes ejercicios, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo dependerán del ejercicio; cada paso indicará cuál debes utilizar.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con las instrucciones actuales. Más adelante, las actualizarás y volverás a enviar el mismo prompt para comparar los resultados.

1. Cierra las pestañas del editor que hayas abierto en ejercicios anteriores para que Copilot solo reciba el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el helper.
3. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Captura del selector de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios, a menudo tanto al archivo helper como a sus pruebas.
6. Fíjate en que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve un array tipado de editoriales. Esto procede de las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** comentarios TSDoc y un encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios de documentación aunque no se lo pidas. No pasa nada: el objetivo sigue siendo mejorar la *coherencia* del resultado al actualizar las instrucciones.

## Añadir un nuevo estándar del repositorio

Como hemos señalado, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Documenta los estándares del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que describe los estándares de programación, pero todavía no contiene reglas para documentar el código; por eso el helper generado no incluía comentarios de documentación.
3. Justo debajo de los estándares existentes, añade estas líneas Markdown para pedir a Copilot que incluya encabezados de comentario de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, puedes crear archivos de instrucciones en el ámbito del repositorio (`.github/copilot-instructions.md`) para establecer directrices generales o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios de documentación que acabas de añadir.

## Repetir el prompt y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva.
2. Selecciona `src/lib/publishers.ts` en el editor para que Copilot se centre en el archivo correcto.
3. Envía el **mismo prompt** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un nuevo estándar del proyecto y lo aplique a código real sobre el que trabajarás en el próximo ejercicio.

## Confirmar y enviar la actualización de instrucciones

Los archivos de instrucciones se gestionan en el control de código fuente como cualquier otro recurso del repositorio. Confirma los cambios y envía la rama.

1. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que la actualización de las instrucciones y los cambios del helper estén presentes:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos que Copilot haya modificado para crear el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de incorporación de cambios

Abre una solicitud de incorporación de cambios y relaciónala con la incidencia sobre los estándares de documentación del backlog. Copilot puede localizar la incidencia, crear una PR para cerrarla y combinarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot realiza acciones en GitHub. La plantilla del proyecto ya configura las herramientas de GitHub en el espacio de trabajo, así que no hay que hacer nada más. La primera vez que Copilot utilice una de ellas, VS Code puede pedirte que inicies sesión en GitHub. Sigue las instrucciones. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y comprueba que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia sobre la actualización de los archivos de instrucciones y cree una PR desde la rama actual. La descripción debe mencionar tanto los cambios de instrucciones como la base del helper de editoriales:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si procede.
6. Cuando se cree la PR, pídele que la combine y que vuelva a `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y comandos de shell, revisa la solicitud y selecciona **Allow** si procede.

¡Ya has creado y combinado una solicitud de incorporación de cambios con GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones del proyecto y has utilizado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*;
- añadir un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`;
- repetir el prompt y observar cómo adopta el código propuesto el nuevo estándar;
- confirmar la actualización de instrucciones y la base del helper en `main` para que el siguiente ejercicio pueda utilizarlas.

A continuación, utilizarás esas instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una función al código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para redactar mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
