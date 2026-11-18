---
title: "Lección 5 - Personalizar y usar una habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escribir código implica mucho más que limitarse a escribirlo. Hemos validado manualmente que funciona y hemos usado archivos de instrucciones para garantizar que cumple nuestros estándares. Pero ¿qué ocurre con las pruebas, el lint y el resto de las tareas de integración continua (CI)?

Para este tipo de tareas, las **habilidades de agente** son la mejor opción. Las habilidades ayudan a Copilot a entender cómo ejecutar correctamente operaciones como estas.

En esta lección:

- explorarás la habilidad `quality-checks` existente.
- personalizarás el formato de sus resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones de tareas reutilizables, scripts ejecutables y recursos auxiliares que un agente carga cuando los necesita. En esencia, son una carpeta con el nombre de la habilidad y un archivo Markdown llamado `SKILL.md`. El archivo Markdown contiene frontmatter con un nombre y una descripción que definen la habilidad, una introducción sobre lo que hace e indicaciones sobre cuándo debe invocarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos que la habilidad utiliza cuando se invoca.

> [!NOTE]
> Una habilidad no necesita carpetas ni archivos adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, porque usa los comandos que ya existen en el proyecto.

Las habilidades pueden estar en la carpeta `.github/skills` de un proyecto para que el equipo pueda compartirlas y reutilizarlas como recursos del repositorio, o en la carpeta de habilidades del usuario, `~/.copilot/skills`.

## Explorar la habilidad

Vamos a explorar la habilidad que creó el equipo de Tailspin Toys para ejecutar las pruebas unitarias, el lint y las comprobaciones de tipos: `quality-checks`.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee `name` y `description` al principio. La descripción ayuda a Copilot a entender cuándo debe invocar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot durante el proceso de pruebas y lint.
4. Comprueba que la habilidad todavía no contiene una sección **Formato de los resultados**.

## Ejecutar la habilidad antes de realizar un cambio

Las habilidades se pueden invocar directamente mediante Copilot CLI o usando lenguaje natural. Pidamos a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Usa este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final.

## Personalizar el informe

Queremos mejorar el informe para que indique qué se ha ejecutado, si se ha completado correctamente y qué han comunicado realmente las herramientas. Actualicemos la habilidad para generar ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade la siguiente sección al final del archivo:

   ```markdown
   ## Formato de los resultados

   Al terminar, indica cada comando que se haya ejecutado y si se completó correctamente, falló o quedó bloqueado. Incluye el número de pruebas, la duración, los errores, las advertencias y otras métricas solo si la herramienta las proporciona. Indica el siguiente paso ante cualquier error o bloqueo y nunca presentes como superada una comprobación omitida o incompleta.
   ```

3. El archivo se guarda automáticamente.

## Ejecutar la habilidad actualizada

Una vez realizado el cambio, veamos cómo funciona. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Introduce:

   ```plaintext
   /skills reload
   ```

2. Usa exactamente el mismo prompt que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final y compáralo con el primero.

## Resumen y pasos siguientes

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- has explorado la habilidad `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos.
- has personalizado el formato de sus resultados.
- has vuelto a cargar y ejecutado la habilidad.

Este cambio se incluirá en la PR de la funcionalidad de filtrado. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y un script de exportación incluido: `drawio`][drawio-example]
- [Pruebas de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
