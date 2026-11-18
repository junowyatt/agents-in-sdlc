---
title: "Ejercicio 0: Requisitos previos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de empezar los ejercicios de VS Code, necesitas preparar todo. Crearás tu propia copia del repositorio Tailspin Toys, crearás un [codespace][codespaces] para trabajar y comprobarás que GitHub Copilot Chat se ejecuta correctamente en el editor.

## Configurar el repositorio del laboratorio

Para crear una copia del repositorio con el código que desarrollarás, crearás una instancia a partir de la [plantilla][template-repository]. La nueva instancia contendrá todos los archivos necesarios para el laboratorio y la utilizarás a lo largo de los ejercicios.

1. En una ventana nueva del navegador, navega a la [plantilla de Tailspin Toys][tailspin-template].
2. Crea tu propia copia del repositorio seleccionando el botón **Use this template** en la página del repositorio del laboratorio. Después, selecciona **Create a new repository**.

    ![Botón Use this template](../../../_images/ex0-use-template.png)

3. Si realizas el taller como parte de un evento dirigido por GitHub o Microsoft, sigue las instrucciones de las personas mentoras. En caso contrario, puedes crear el repositorio nuevo en una organización en la que tengas acceso a GitHub Copilot.

    ![Introducir la configuración del repositorio a partir de la plantilla](../../../_images/ex0-repository-settings.png)

4. Anota la ruta del repositorio que hayas creado (**nombre-de-organización-o-usuario/nombre-del-repositorio**), porque la utilizarás más adelante en el laboratorio.

> [!NOTE]
> **Tu backlog está listo**
>
> Cuando creas el repositorio a partir de la plantilla, se crea automáticamente una lista de incidencias pendientes. Trabajarás con ellas a lo largo del taller: no tienes que crear ninguna.

## Crear un codespace

A continuación, utilizarás un codespace para completar los ejercicios del laboratorio.

[GitHub Codespaces][codespaces] es un entorno de desarrollo basado en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un entorno de desarrollo integrado con soporte para varios lenguajes de programación, extensiones y herramientas.

1. Navega al repositorio recién creado.
2. Selecciona el botón verde **Code**.

    ![Seleccionar el botón Code](../../../_images/ex0-code-button.png)

3. Selecciona la pestaña **Codespaces** y selecciona el botón **+** para crear un codespace nuevo.

    ![Crear un codespace nuevo](../../../_images/ex0-create-codespace.png)

La creación del codespace tardará varios minutos, aunque sigue siendo mucho más rápido que instalar manualmente todos los servicios. Puedes aprovechar este tiempo para explorar otras funciones de GitHub Copilot; nos centraremos en ellas a continuación.

> [!CAUTION]
> Volverás al codespace en un ejercicio posterior. Por ahora, déjalo abierto en una pestaña del navegador.

> [!NOTE]
> Este taller está diseñado para ejecutarse dentro de un codespace o de un [dev container][dev-containers] local. Ambos garantizan que el entorno tenga todos los requisitos previos necesarios para una experiencia fluida. Si prefieres ejecutarlo localmente, abre el repositorio clonado en VS Code y selecciona **Reopen in Container** cuando se te solicite. VS Code creará el mismo contenedor de desarrollo que utiliza el codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Utilizar GitHub Copilot Chat y el modo agente

Para acceder al modo agente de GitHub Copilot Chat, tienes que tener instalada la extensión GitHub Copilot Chat en el IDE. Esto ya debería ser así si utilizas GitHub Codespaces.

> [!TIP]
> Si no tienes instalada la extensión GitHub Copilot Chat, puedes [instalarla desde Visual Studio Marketplace][copilot-chat-extension]. También puedes abrir la vista Extensions en Visual Studio Code, buscar **GitHub Copilot Chat** y seleccionar **Install**.

Cuando tengas instalada la extensión, es posible que tengas que autenticarte con tu cuenta de GitHub para activarla.

1. Vuelve al codespace.
2. Si no ves Copilot Chat a la derecha del editor, selecciona el icono de **Copilot Chat** en la parte superior del codespace.
3. Escribe un mensaje como «Hello world» en la ventana de Copilot Chat y pulsa Enter. Esto debería activar Copilot Chat.
4. Como alternativa, si no estás autenticado, se te pedirá que inicies sesión en tu cuenta de GitHub. Sigue las instrucciones para autenticarte.

    ![Ejemplo de solicitud de autenticación de Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Después de autenticarte, debería aparecer la ventana de Copilot Chat.

## Resumen

¡Enhorabuena, has creado una copia del repositorio del laboratorio! También has iniciado el proceso de creación de tu codespace, que utilizarás cuando empieces a escribir código.

## Próximo paso

Vamos a empezar a poner Copilot a trabajar. Continúa con el [Ejercicio 1 - Instrucciones personalizadas][next-lesson], donde enseñarás a Copilot las convenciones de tu proyecto.

## Recursos

- [Información general de GitHub Codespaces][codespaces]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Primeros pasos con Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
