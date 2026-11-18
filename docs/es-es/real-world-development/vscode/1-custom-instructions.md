---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Próxima lección: Modo agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una manera determinada —o hay información general que Copilot debería conocer— conviene garantizar que ese contexto esté accesible. Hay varias maneras de compartir contexto específico con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que permiten proporcionar orientación sobre la generación de código.

En este ejercicio, harás lo siguiente:

- explorar cómo el contexto específico del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones con ámbito por ruta.
- generar el primer fragmento de datos para el filtrado —una función auxiliar de editores— con las instrucciones existentes.
- añadir un nuevo estándar global del repositorio a `.github/copilot-instructions.md`.
- volver a ejecutar la misma indicación y observar cómo el código generado adopta el nuevo estándar.
- confirmar la actualización de las instrucciones y el fragmento de filtrado en `main` para que Copilot pueda utilizar la orientación actualizada en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede diferir de algunos estándares establecidos. Copilot no es determinista. El objetivo de este ejercicio es observar la tendencia del cambio de comportamiento después de actualizar las instrucciones, no reproducir exactamente el resultado carácter por carácter.

## Archivos de instrucciones

### Escenario

Como corresponde a un buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Estos incluyen:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe utilizar un tema oscuro y tener un aspecto moderno.
- La documentación debe añadirse al código mediante comentarios de documentación TSDoc.
- Debe añadirse un bloque de comentarios al principio de cada archivo que describa su función.

Mediante los archivos de instrucciones garantizarás que Copilot tenga la información adecuada para realizar las tareas conforme a las prácticas destacadas.

### Instrucciones personalizadas

Las instrucciones personalizadas permiten proporcionar contexto y preferencias a Copilot para que comprenda mejor tu estilo de programación y tus requisitos. Esta función puede ayudarte a orientar Copilot y obtener sugerencias y fragmentos de código más relevantes. Puedes especificar convenciones de programación preferidas, bibliotecas o incluso los tipos de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos de archivos específicos como contexto de una tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo de instrucciones enviado a Copilot en todas las solicitudes del repositorio. Debe contener información a nivel de proyecto —contexto relevante para la mayoría de las solicitudes de chat o CLI enviadas a Copilot—. Puede incluir la pila tecnológica utilizada, una descripción general de lo que se está desarrollando, buenas prácticas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` se pueden crear para tareas o tipos de archivos concretos. Puedes utilizarlos para proporcionar directrices sobre lenguajes específicos, como TypeScript o Astro, o sobre tareas como crear un componente de interfaz o un conjunto nuevo de pruebas unitarias.

> [!NOTE]
> Cuando trabajas en tu IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para completar código o realizar sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan tanto los archivos a nivel de repositorio como los archivos `*.instructions.md` —con frontmatter `applyTo`— al generar código.
>
> Por último, Copilot [admite archivos de instrucciones que siguen otros estándares][custom-instructions-support], incluidos los archivos AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

Una conversación completa sobre cómo crear archivos de instrucciones excede el alcance del taller. Los ejemplos incluidos en el proyecto muestran un enfoque representativo. En términos generales:

- Mantén las instrucciones de `copilot-instructions.md` centradas en la orientación del proyecto, como una descripción de lo que se está desarrollando, la estructura del proyecto y los estándares globales de programación.
- Usa archivos `*.instructions.md` para proporcionar instrucciones específicas de tipos de archivo —pruebas unitarias, componentes de Astro, la capa de datos— o de tareas concretas.
- Usa lenguaje natural. Mantén las directrices claras y proporciona ejemplos de cómo debería verse el código y cómo no debería verse.

No existe una única manera de crear archivos de instrucciones, igual que no existe una única manera de utilizar la IA. Mediante la experimentación descubrirás qué funciona mejor en tu proyecto.

> [!TIP]
> Todos los proyectos que utilizan GitHub Copilot deberían tener una colección sólida de archivos de instrucciones. Al explorar los que incluye este proyecto, verás archivos para numerosos tipos de tareas, incluidas [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudar a generar archivos de instrucciones. Cada interfaz lo expone de otra manera —por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI— y la lección correspondiente lo indicará cuando sea relevante.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio lleno de archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en el repositorio: hay un archivo principal `copilot-instructions.md` y una colección de archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y observa la breve descripción del proyecto junto con secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa la guía anidada **GitHub Actions Workflows**. Se aplica a todas las interacciones que tengas con Copilot.
3. Abre la carpeta `.github/instructions` y examínala. Verás instrucciones para archivos de Astro, la capa de datos de Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: establece un patrón global —relativo a la raíz del repositorio— que determina a qué archivos se aplican las instrucciones. Aquí, cualquier archivo de prueba de TypeScript —por ejemplo, uno que coincida con `**/*.test.ts`— tendrá correspondencia.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate al final. Observa los enlaces a otros archivos de instrucciones —como `unit-tests.instructions.md`— y a archivos existentes del proyecto. Esto permite dividir conjuntos amplios de instrucciones en archivos reutilizables más pequeños y señalar a Copilot ejemplos que debe seguir al generar código. Las rutas que aparecen son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige documentación dentro del código. En los próximos pasos añadirás reglas para comentarios de documentación TSDoc y encabezados de comentario en los archivos.

## Crear una rama para los cambios

Vamos a seguir algunas buenas prácticas y crear una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea y cambia a una rama nueva:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de iniciar los ejercicios siguientes, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían en cada ejercicio; cada paso lo indicará cuando sea necesario.

Para ver el impacto de las instrucciones personalizadas, empieza enviando una indicación con el archivo de instrucciones actual. Después lo actualizarás y volverás a enviar la misma indicación para comprobar la diferencia.

1. Cierra las pestañas abiertas del editor de ejercicios anteriores para que Copilot solo tome el contexto que quieras.
2. Abre `src/lib/publishers.ts` para que Copilot sepa dónde debe vivir la función auxiliar.
3. Selecciona **Agent** en el selector de agentes de la vista de Chat para que Copilot pueda aplicar cambios a los archivos.

   ![Captura de pantalla del selector de agentes de la vista de Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía la siguiente indicación:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica las modificaciones de código, normalmente tanto a la función auxiliar como a sus pruebas.
6. Observa que la función auxiliar propuesta es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores. Esto procede de las convenciones de capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que el código propuesto **no contiene** comentarios de documentación TSDoc ni un encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, existe la posibilidad de que añada comentarios de documentación aunque no se lo hayas indicado. Si ocurre, no pasa nada: el objetivo sigue siendo observar la mejora de consistencia después de actualizar las instrucciones.

## Añadir un nuevo estándar del repositorio

Como hemos destacado antes, `.github/copilot-instructions.md` está diseñado para proporcionar información a nivel de proyecto a Copilot. Vamos a asegurarnos de que los estándares de programación del repositorio estén documentados para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería encontrarse cerca de la línea 27. Observa que documenta los estándares de programación del proyecto, pero todavía no contiene una regla para documentación dentro del código, razón por la cual la función auxiliar generada no tenía comentarios de documentación.
3. Añade las siguientes líneas Markdown justo debajo de los estándares existentes para indicar a Copilot que añada encabezados de comentario de archivo y comentarios de documentación TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, los archivos de instrucciones pueden crearse a nivel de repositorio —`.github/copilot-instructions.md`— para directrices globales o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo a nivel de repositorio es el lugar adecuado para estándares globales del proyecto, como la regla de comentarios de documentación que acabas de añadir.

## Volver a ejecutar la indicación y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar el contexto.
2. Vuelve a `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía la **misma indicación** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto comienza ahora con un bloque de comentarios parecido a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta incluye ahora un comentario TSDoc parecido a:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Acabas de orientar a Copilot para que siga un nuevo estándar del proyecto y lo aplique a código real sobre el que se apoyará el siguiente ejercicio.

## Confirmar las instrucciones y enviar la rama

Los archivos de instrucciones son como cualquier otro recurso del repositorio, lo que significa que se gestionan mediante el mismo enfoque de control de versiones que cualquier otro elemento. Vamos a confirmar y enviar la rama al repositorio.

1. Abre una ventana nueva de terminal pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Desde el terminal, confirma que están presentes la actualización de las instrucciones y los cambios de la función auxiliar ejecutando:

   ```bash
   git status
   ```

3. Desde el terminal, añade al área de preparación y confirma la actualización de las instrucciones y todos los archivos que Copilot haya modificado para establecer la base de la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otras modificaciones compatibles (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Desde el terminal, envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de incorporación de cambios

Con la rama enviada, debemos crear una solicitud de incorporación de cambios y vincularla a la incidencia del backlog relacionada con el estándar de documentación. Podríamos hacerlo manualmente, pero Copilot puede hacerlo por nosotros mediante las herramientas de GitHub que ya están conectadas al proyecto. Vamos a pedirle que encuentre la incidencia, cree la PR para cerrarla y después la combine.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actuará en GitHub por ti. La plantilla del proyecto ya conecta las herramientas de GitHub al espacio de trabajo, así que no tienes que configurar nada. La primera vez que Copilot utilice una, es posible que VS Code te pida que inicies sesión en GitHub. Sigue las indicaciones para permitirlo. Aprenderás cómo funciona esta conexión —Model Context Protocol— en un ejercicio posterior.

1. Abre Copilot Chat dentro del codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista de Copilot Chat y comprueba que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que encuentre la incidencia relacionada con la actualización de archivos de instrucciones y cree una PR desde la rama actual que describa tanto las actualizaciones de instrucciones como la nueva base de la función auxiliar de editores:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot empezará a buscar la incidencia y crear la PR.
5. Cuando se te solicite **Allow** para que Copilot pueda realizar acciones de GitHub en tu nombre, revisa el comando y selecciona **Allow** si procede.
6. Cuando se haya creado la PR, pide a Copilot que la combine y vuelva a situar tu rama en main con la siguiente indicación:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando se te solicite **Allow** para que Copilot pueda realizar acciones de GitHub y ejecutar comandos de terminal en tu nombre, revisa el comando y selecciona **Allow** si procede.

¡Ya has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y próximos pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones de este proyecto y después has utilizado Copilot Chat en VS Code para:

- enviar una indicación de generación de código y observar qué produce Copilot con las instrucciones existentes.
- añadir un nuevo estándar global del repositorio a `.github/copilot-instructions.md`.
- volver a ejecutar la misma indicación y observar cómo el código propuesto adopta el nuevo estándar.
- confirmar la actualización de las instrucciones y la base de la función auxiliar en `main` para que el siguiente ejercicio pueda apoyarse en ellas.

A continuación, pondrás esas instrucciones a trabajar en el [modo agente][next-lesson] mientras Copilot añade una funcionalidad nueva en todo el código base.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot — una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
