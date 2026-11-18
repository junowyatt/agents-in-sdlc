---
title: "Lección 4 - Orientar Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código del filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene que Copilot tenga esa orientación disponible. Los [archivos de instrucciones][instruction-files] describen no solo qué código quieres, sino cómo debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que Copilot ha utilizado, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección, harás lo siguiente:

- explorar cómo las instrucciones del repositorio y los archivos de instrucciones con ámbito por ruta llegan al agente.
- actualizar el archivo de instrucciones para garantizar que se respeten los estándares de programación.
- observar el impacto de los archivos de instrucciones en el código.

## Escenario

Como corresponde a un buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Estos incluyen:

- Los comentarios deben explicar la intención y las decisiones no evidentes, en lugar de repetir el código.
- Las funciones exportadas en `db/` y `src/lib/` deben documentar su propósito, parámetros y valores de retorno con TSDoc/JSDoc, incluida un argumento `db` inyectable cuando exista.
- Los componentes reutilizables de Astro deben documentar sus contratos de `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Deben conservarse las instrucciones existentes de formato y lint.

Mediante los archivos de instrucciones garantizarás que Copilot tenga la información adecuada para realizar las tareas conforme a las prácticas destacadas.

## Archivos de instrucciones

Las instrucciones personalizadas permiten proporcionar contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta función puede ayudarte a orientar Copilot y obtener sugerencias y fragmentos de código más relevantes. Puedes especificar convenciones de programación preferidas, bibliotecas o incluso los tipos de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos de archivos específicos como contexto de una tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo de instrucciones enviado a Copilot en todas las solicitudes del repositorio. Debe contener información del proyecto relevante para la mayoría de las solicitudes.
- Archivos `.github/instructions/*.instructions.md`, que proporcionan directrices para lenguajes, tipos de archivos o tareas concretas.

> [!NOTE]
> Otros formatos de instrucciones y su compatibilidad varían según el entorno. Consulta la [referencia de compatibilidad de instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explorar los archivos de instrucciones personalizadas de este proyecto

Para empezar, ya se ha incluido un conjunto de archivos de instrucciones en el proyecto de inicio. Vamos a ver qué contienen antes de realizar un cambio y observar su efecto.

1. Vuelve al codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Explora el archivo y observa la breve descripción del proyecto y sus directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en el repositorio.
4. Abre la carpeta `.github/instructions` y explora los archivos. Verás instrucciones para archivos de Astro, la capa de datos de Drizzle, pruebas y más.
5. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: establece un patrón global que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa sus enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Esto permite dividir conjuntos amplios de instrucciones en archivos reutilizables más pequeños y señalar ejemplos que Copilot debe seguir.

## Actualizar los archivos de instrucciones con las directrices del equipo

Los archivos existentes son un buen comienzo, pero todavía hay una diferencia. Vamos a modificar el archivo principal `copilot-instructions.md` para asegurar que Copilot añada comentarios TSDoc al TypeScript generado.

1. En `.github/copilot-instructions.md`, busca la sección titulada **Code formatting guidance**, que debería encontrarse alrededor de la línea 35.
2. Añade lo siguiente como último elemento de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

¡El archivo se guardará automáticamente!

## Usar la orientación actualizada

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones actualizadas, sal y reanuda la conversación de filtrado para que Copilot comience con la nueva orientación sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado todavía está abierta, introduce `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se solicita, selecciona la conversación de filtrado y confirma que la rama `game-filters-cli` está activa.
3. Pide a Copilot que aplique la orientación actualizada:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Introduce `/diff` y lee los archivos TypeScript modificados. Observa los nuevos comentarios TSDoc y confirma que explican el código con precisión.

## Resumen y próximos pasos

Has explorado cómo Copilot CLI obtiene contexto de los archivos de instrucciones y has aplicado un estándar nuevo a tu funcionalidad. En concreto:

- has explorado cómo las instrucciones del repositorio y los archivos de instrucciones con ámbito por ruta llegan al agente.
- has actualizado el archivo de instrucciones para garantizar que se respeten los estándares de programación.
- has observado el impacto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para garantizar que el lint y las pruebas se ejecutan de forma constante.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot — una colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
