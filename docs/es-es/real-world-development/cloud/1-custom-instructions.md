---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Próxima lección: Agente en la nube de Copilot →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una manera determinada —o hay información general que Copilot debería conocer— conviene garantizar que ese contexto esté accesible. Los [archivos de instrucciones][instruction-files] permiten proporcionar esa orientación para que Copilot entienda no solo qué quieres hacer, sino cómo quieres hacerlo.

En este ejercicio, harás lo siguiente:

- explorar cómo el contexto específico del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones con ámbito por ruta.
- añadir un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los entornos de VS Code y CLI, aquí no ejecutarás una indicación *antes/después*: el agente en la nube de Copilot trabaja de forma asincrónica sobre incidencias de GitHub, por lo que el efecto es más difícil de comparar en tiempo real. Verás la influencia del archivo de instrucciones más adelante en este entorno, cuando revises las solicitudes de incorporación de cambios generadas por el agente en la nube.

## Archivos de instrucciones

### Escenario

Como corresponde a un buen equipo de desarrollo, Tailspin Toys tiene un conjunto de directrices y requisitos para las prácticas de desarrollo. Estos incluyen:

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

Una conversación completa sobre cómo crear archivos de instrucciones excede el alcance de este taller. Sin embargo, los ejemplos incluidos en el proyecto de ejemplo muestran un enfoque representativo. En términos generales:

- Mantén las instrucciones de `copilot-instructions.md` centradas en la orientación del proyecto, como una descripción de lo que se está desarrollando, la estructura del proyecto y los estándares globales de programación.
- Usa archivos `*.instructions.md` para proporcionar instrucciones específicas de tipos de archivo —pruebas unitarias, componentes de Astro o la capa de datos— o de tareas concretas.
- Usa lenguaje natural. Mantén las directrices claras y proporciona ejemplos de cómo debería verse el código y cómo no debería verse.

No existe una única manera de crear archivos de instrucciones, igual que no existe una única manera de utilizar la IA. Mediante la experimentación descubrirás qué funciona mejor en tu proyecto.

> [!TIP]
> Todos los proyectos que utilizan GitHub Copilot deberían tener una colección sólida de archivos de instrucciones. Cuando exploras los que incluye este proyecto, verás archivos para numerosos tipos de tareas, incluidas [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
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
2. Explora el archivo y observa la breve descripción del proyecto junto con secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa la guía anidada **GitHub Actions Workflows**. Se aplica a las interacciones que tengas con Copilot.
3. Abre la carpeta `.github/instructions` y examínala. Verás instrucciones para archivos de Astro, la capa de datos de Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: establece un patrón global —relativo a la raíz del repositorio— que determina a qué archivos se aplican las instrucciones. Aquí, cualquier archivo de prueba de TypeScript —por ejemplo, uno que coincida con `**/*.test.ts`— tendrá correspondencia.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate al final. Observa los enlaces a otros archivos de instrucciones —como `unit-tests.instructions.md`— y a archivos existentes del proyecto. Esto permite dividir conjuntos amplios de instrucciones en archivos reutilizables más pequeños y señalar a Copilot ejemplos que debe seguir al generar código. Las rutas que aparecen son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige documentación dentro del código. En los próximos pasos añadirás reglas para comentarios de documentación TSDoc y encabezados de comentario en los archivos.

## Añadir un nuevo estándar del repositorio

El siguiente paso es la única edición que realizarás aquí: añade una regla global que exija que la documentación viva en el código como comentarios de documentación TSDoc y como encabezado de comentario a nivel de archivo. El agente en la nube recogerá esta información cuando trabaje en las incidencias que le asignes más adelante en este entorno.

Antes de editar, prepara una rama (el agente en la nube leerá las instrucciones desde cualquier rama a la que apunte la incidencia, pero de todas formas confirmarás los cambios de forma limpia):

1. Desde el terminal del codespace, crea y cambia a una rama nueva:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como hemos destacado antes, `.github/copilot-instructions.md` está diseñado para proporcionar información a nivel de proyecto a Copilot. Vamos a asegurarnos de que los estándares de programación del repositorio estén documentados para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería encontrarse cerca de la línea 27. Observa cómo documenta los estándares de programación del proyecto, pero todavía no contiene una regla para documentación dentro del código, razón por la cual el generador creado no tenía comentarios de documentación.
3. Añade las siguientes líneas Markdown justo después de los estándares existentes para indicar a Copilot que añada encabezados de comentario de archivo y comentarios de documentación TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, los archivos de instrucciones pueden crearse a nivel de repositorio —`.github/copilot-instructions.md`— para directrices globales o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo a nivel de repositorio es el lugar adecuado para estándares globales del proyecto, como la regla de comentarios de documentación que acabas de añadir.

## Confirmar, enviar y combinar la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones desde la rama a la que apunta la incidencia. Cuando asignes una incidencia a Copilot en el siguiente ejercicio, Copilot partirá de `main`, así que los cambios de instrucciones deben llegar a `main` para que el agente pueda recogerlos.

1. Añade y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. Abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` hacia `main` en github.com y combínala. Después, el agente en la nube leerá estas instrucciones cuando trabaje en las incidencias asignadas en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` para este taller, puedes omitir la rama y confirmar directamente en `main`. El paso de rama se incluye para que el taller refleje la forma de trabajar en un proyecto real.

## Resumen y próximos pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones de este proyecto y has añadido un nuevo estándar global a `.github/copilot-instructions.md`. Verás ese estándar aplicado en las solicitudes de incorporación de cambios que genere el agente en la nube a lo largo del resto de este entorno.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
