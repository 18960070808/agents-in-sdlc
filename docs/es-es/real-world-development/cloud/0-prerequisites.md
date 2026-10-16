---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios del agente de GitHub Copilot en la nube, prepara todo lo necesario. Crearás tu propia copia del repositorio Tailspin Toys y pondrás en marcha un [codespace][codespaces] que podrás usar para editar archivos de instrucciones y revisar el trabajo del agente en la nube.

## Configurar el repositorio del taller

Para crear una copia del repositorio en la que desarrollarás el código, crearás una instancia a partir del [repositorio de plantilla][template-repository]. La nueva instancia contendrá todos los archivos necesarios para el taller y la utilizarás durante los ejercicios.

1. En una nueva ventana del navegador, ve a la [plantilla de Tailspin Toys][tailspin-template].
2. Crea tu propia copia del repositorio seleccionando el botón **Use this template** en la página del repositorio del taller. Después, selecciona **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si participas en el taller como parte de un evento dirigido por GitHub o Microsoft, sigue las instrucciones de los mentores. De lo contrario, puedes crear el nuevo repositorio en una organización en la que tengas acceso a GitHub Copilot.

    ![Configuración de la plantilla del repositorio](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que has creado (**nombre-de-organización-o-usuario/nombre-del-repositorio**), ya que la necesitarás más adelante en el taller.

## Crear un codespace

A continuación, utilizarás un codespace para completar los ejercicios del taller.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que te permite escribir, ejecutar y depurar código directamente en el navegador. Ofrece un IDE completo compatible con varios lenguajes de programación, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona el botón verde **Code**.

    ![Seleccionar el botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y, después, el botón **+** para crear un codespace.

    ![Crear un codespace](../../../_images/ex0-create-codespace.png)

La creación del codespace tardará varios minutos, aunque sigue siendo mucho más rápida que instalar manualmente todos los servicios. Mientras tanto, puedes explorar otras funciones de GitHub Copilot que veremos a continuación.

> [!CAUTION]
> Volverás al codespace en un ejercicio posterior. De momento, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está diseñado para ejecutarse en un codespace o en un [contenedor de desarrollo local][dev-containers]. Ambos entornos incluyen todos los requisitos previos necesarios para que puedas trabajar sin problemas. Si prefieres trabajar localmente, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando aparezca el aviso; VS Code compilará el mismo contenedor de desarrollo que utiliza el codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Resumen

Enhorabuena, ya has creado una copia del repositorio del taller. También has iniciado la creación del codespace que utilizarás para trabajar con el agente de GitHub Copilot en la nube.

## Siguiente paso

Añade instrucciones personalizadas que seguirá el agente en la nube. Continúa con el [Ejercicio 1: Instrucciones personalizadas][next-lesson].

## Recursos

- [Información general sobre GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Primeros pasos con Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
