---
title: "Ejercicio 2 - Añadir una funcionalidad nueva con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Hasta las actualizaciones más sencillas de una aplicación suelen requerir modificar varios archivos y ejecutar operaciones como pruebas. Como desarrollador, el flujo habitual consiste en localizar todos los archivos necesarios, realizar los cambios, ejecutar las pruebas, depurar, detectar el archivo que falta, hacer otra actualización... La lista continúa.

Aquí es donde entra en juego el modo agente de Copilot.

El modo agente de Copilot está diseñado para actuar de forma más autónoma en el IDE. Se comporta de manera similar a una persona desarrolladora: empieza explorando la estructura del proyecto existente, realiza las modificaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente cualquier error que detecte. Vamos a explorar cómo utilizar el modo agente para introducir una funcionalidad nueva en el sitio.

> [!NOTE]
> Aunque los nombres sean parecidos, el modo agente y el agente en la nube están diseñados para experiencias distintas. El modo agente realiza las tareas en el IDE, lo que permite ciclos rápidos de comentarios e interacción. El agente en la nube está diseñado como una persona desarrolladora compañera: trabaja de forma asincrónica, como un miembro del equipo, e interactúa contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot puede explorar el proyecto, identificar los archivos relevantes y realizar cambios coordinados.
- el modo agente de GitHub Copilot puede implementar nuevas funciones en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de combinarlos con el código base.

## Escenario

A medida que crece la lista de juegos, quieres permitir a los usuarios filtrar por categoría y editor. Ya has añadido una función auxiliar de editores en el ejercicio anterior y ahora completarás el trabajo restante de la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecutar el sitio web Tailspin Toys

Antes de realizar cambios, vamos a explorar el sitio web Tailspin Toys para entender su funcionalidad actual.

El sitio es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite a los usuarios listar juegos y mostrar detalles sobre ellos. Se trata de una única aplicación Astro que representa sus páginas como HTML estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local en el frontmatter mediante Drizzle ORM: no hay una API de servidor independiente ni un framework de interfaz en el cliente. Las funciones reutilizables de acceso a datos viven en `src/lib/` y cualquier interactividad se añade mediante un elemento `<script>` pequeño y limitado de Astro que utiliza API DOM estándar.

### Iniciar el sitio web

Para facilitar la ejecución del sitio, se proporciona una tarea `npm run dev` que inicia el único servidor de desarrollo de Astro. Puedes ejecutarla en GitHub Codespaces siguiendo estos pasos:

1. Vuelve al codespace. Deberías haber vuelto a `main` después del ejercicio anterior.
2. Abre una ventana nueva de terminal dentro del codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y cambia a una rama nueva para este trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el siguiente comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté preparado, verás un banner que indica la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir una ventana del navegador en `http://localhost:4321`, ciérralo seleccionando la **x**.

5. Abre el sitio manteniendo pulsadas <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor de Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar un enlace a la URL localhost desde el terminal de Codespaces se redirige automáticamente a `https://<nombre-del-codespace>-4321.app.github.dev/`. Se trata de un túnel privado hacia el codespace, que ahora aloja tu servidor web.

### Explorar el sitio web

Cuando el sitio esté ejecutándose, puedes explorar su funcionalidad. Las funciones principales incluyen:

- **Home Page**: muestra una lista de juegos de mesa con títulos, imágenes y descripciones.
- **Game Details Page**: al seleccionar un juego, se abre una página de detalles con más información sobre el juego, incluidos título, descripción, editor y categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de iniciar los ejercicios siguientes, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modelo y el modo varían en cada ejercicio; cada paso lo indicará cuando sea necesario.

La implementación inicial del sitio funciona, pero queremos mejorarlo añadiendo nuevas capacidades. Vamos a empezar revisando el backlog. Cuando configuraste el proyecto a partir de la plantilla, se creó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esos elementos.

1. Selecciona **Agent** en el selector de agentes de la vista de Chat. El agente **Agent** planifica e implementa cambios entre archivos de forma autónoma, ejecuta comandos de terminal y utiliza herramientas.

   ![Captura de pantalla del selector de agentes de la vista de Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no indican ninguna preferencia por un modelo concreto. Al crear este taller usamos Claude Sonnet 4.5 y por eso lo incluimos en las instrucciones. Esperamos que las sugerencias de código sean relativamente coherentes para garantizar una buena experiencia. Sin embargo, como los grandes modelos de lenguaje son probabilísticos, puede que las sugerencias recibidas difieran de las indicadas en el taller. Esto es completamente normal y esperado.

> [!NOTE]
> Debido a la naturaleza probabilística de los grandes modelos de lenguaje, Copilot puede utilizar un comando MCP distinto, pero debería poder completar la tarea.

3. Pregunta a Copilot por el backlog de incidencias enviándole la siguiente indicación:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando y listar todas las incidencias.
5. Revisa la lista generada de incidencias.

Observa cómo Copilot también ha priorizado los elementos según los que considera más útiles para las personas usuarias.

## Revisar los archivos de instrucciones

Antes de activar el agente para generar el código, conviene revisar el archivo de instrucciones que utilizarás para proporcionar contexto a Copilot. Aprovecharás el archivo de [interfaz de usuario (UI)][ui-instructions], que contiene contexto sobre cómo añadir funcionalidades al sitio.

1. En el codespace, navega a `.github/instructions/ui.instructions.md`.
2. Observa la orientación general sobre cómo abordar la incorporación de funcionalidades. Incluye:
   - Una descripción general de la arquitectura.
   - Principios de diseño de componentes, comprobabilidad y accesibilidad.
   - Enlaces a archivos de instrucciones específicos para distintos tipos de archivo, incluidos:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia tanto a otros archivos de instrucciones como a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Esto permite reutilizar contenido, dividir instrucciones complejas en partes más manejables y proporcionar ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar el filtrado, habrá que realizar al menos tres modificaciones distintas en la aplicación:

- Añadir o perfeccionar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- Añadir o actualizar las pruebas del comportamiento de filtrado.
- Actualizar la página de listado de juegos para introducir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superar las comprobaciones antes de combinar los cambios con el código base. ¡El modo agente de Copilot puede realizar estas tareas por ti! Vamos a añadir la funcionalidad.

1. Puedes continuar en la conversación actual con Copilot o iniciar una nueva seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de un ejemplo de selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el selector de agentes de la vista de Chat.

   ![Captura de pantalla del selector de agentes de la vista de Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog usando la siguiente indicación:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot empieza explorando el proyecto y localizando los archivos asociados a la funcionalidad deseada. Verás que encuentra las funciones auxiliares de la capa de datos y la interfaz, así como las pruebas. Después empezará a modificar los archivos y ejecutar las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Verás que Copilot realiza varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Puede tardar unos minutos según la complejidad de la tarea y del código base. Durante el proceso pueden aparecer botones **Keep** y **Undo** en el editor. Cuando Copilot termine, tendrás un botón **Keep** o **Undo** para todos los cambios, así que no necesitas seleccionarlos mientras el trabajo está en curso.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla del cuadro de diálogo del panel de Copilot Chat que pide confirmar la ejecución de pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede haber pausas e incluso pruebas que fallen durante el proceso. ¡Es normal! Copilot alternará entre generación de código y pruebas hasta completar la tarea y no detectar errores.

   ![Captura de pantalla de una sesión completa de chat con el modo agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o por cualquier herramienta de IA.

10. Vuelve al navegador con el sitio en ejecución. ¡Explora la nueva funcionalidad!
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y próximos pasos

¡Enhorabuena! En este ejercicio hemos explorado cómo utilizar el modo agente de GitHub Copilot para añadir nuevas capacidades al sitio Tailspin Toys. Hemos aprendido cómo:

- el modo agente de GitHub Copilot puede implementar nuevas funciones en la interfaz y la capa de datos.
- el modo agente de Copilot puede explorar el proyecto, identificar los archivos relevantes y realizar cambios coordinados.
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de combinarlos con el código base.

Ahora vamos a [probar la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrir una solicitud de incorporación de cambios.

### Ejercicio opcional de ampliación: implementar paginación

A medida que crece la lista de juegos será necesario habilitar la paginación. Con las habilidades aprendidas en este ejercicio, pide a Copilot que actualice el sitio para implementar la paginación. Algunas cuestiones que conviene considerar:

- sigue las buenas prácticas existentes, incluidos los archivos de instrucciones.
- decide cómo quieres implementar la paginación y si quieres permitir a la persona usuaria elegir el tamaño de página o dejarlo fijo.
- al crear la indicación, proporciona a Copilot la orientación necesaria para construir la implementación según tus preferencias.
- puede que tengas que iterar con GitHub Copilot, solicitar cambios y aportar contexto. Este es el flujo normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
