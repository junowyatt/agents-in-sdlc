---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para desarrollar el filtrado desde una incidencia y el modo Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una funcionalidad pequeña al proyecto. Sin embargo, los cambios mayores requieren un proceso más sólido. Por suerte, GitHub Copilot CLI está diseñado para integrarse con el flujo existente de una organización y garantizar que creemos las funcionalidades adecuadas de la manera correcta. Esta es la primera de varias lecciones en las que seguirás un proceso de desarrollo típico dirigido por agentes: empezarás con una incidencia para generar una funcionalidad nueva, comprobarás que el código sea válido, confirmarás que la funcionalidad se comporta como se espera y, finalmente, que se combina correctamente con el proyecto.

> [!NOTE]
> Utilizarás la misma conversación y rama mientras avanzas por el flujo de la funcionalidad. Normalmente tendrías ramas o PR distintas para los diferentes tipos de archivos, pero tomaremos un atajo para centrarnos en los conceptos principales.

En esta lección, harás lo siguiente:

- iniciar una conversación nueva de Copilot desde una incidencia de GitHub.
- definir los requisitos en el modo Plan.
- implementar la funcionalidad nueva con el modo Autopilot.
- revisar el código.
- validar manualmente la funcionalidad en un navegador reenviado.

Mientras continúas con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan acotar los juegos por categoría y editor. La incidencia pendiente describe la funcionalidad, pero algunos detalles, como la combinación de categorías, deben acordarse antes de programar. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

La incorporación de agentes de programación con IA al flujo de desarrollo no cambia los fundamentos. Si acaso, ¡los hace aún más importantes! La mayoría de desarrolladores sigue un flujo similar a este:

1. Empezar desde una incidencia registrada que describe lo que hay que hacer.
2. Crear un plan de lo que hay que desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Una vez revisado el código y completado correctamente el proceso de integración continua, combinar el código.

> [!NOTE]
> Los detalles exactos pueden variar según tu equipo y organización, pero la mayoría seguirá una versión del proceso anterior.

Al mantener este enfoque estándar, garantizas que el código generado por IA cumple los requisitos establecidos y pasa el mismo proceso de revisión que el código escrito a mano.

## Modos de conversación

El **modo de conversación** controla el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: tú y el agente trabajáis juntos. El agente propone cambios y espera tu intervención antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas y repite el proceso hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y usa Autopilot para implementarlo.

## Empezar desde la incidencia

Antes de iniciar el trabajo de filtrado, vuelve al codespace y comprueba que el repositorio y el terminal están preparados.

1. Vuelve al codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha combinado la PR de la valoración por estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para destacar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia a la indicación y volver a la pestaña **Session**.

Observa que la indicación empieza con `#7` (o un número parecido). El símbolo `#` permite incluir una incidencia o solicitud de incorporación de cambios (PR) de GitHub en el contexto.

## Planificar la funcionalidad de filtrado

La planificación te permite acordar el enfoque que se seguirá para implementar una funcionalidad o realizar tareas antes de entregárselo a Copilot. Siempre conviene dedicar un poco de tiempo a planificar cualquier trabajo complejo. Vamos a cambiar al modo Plan y pedir a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador de modo situado debajo de la indicación muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, introduce la siguiente indicación:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empezará a crear el plan. Primero explorará el proyecto y, después, determinará el mejor enfoque.

3. Durante el proceso, Copilot puede hacer preguntas sobre el funcionamiento de las capacidades de filtrado. Respóndele según tus preferencias. ¡Aquí no hay respuestas incorrectas!
4. Cuando el plan esté completo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliar el plan.
5. Desplázate hacia arriba y hacia abajo para revisar el plan.
6. Pide a Copilot que revise cualquier parte del plan que no coincida con tus decisiones.

## Aprobar Autopilot

Ya tienes el plan escrito y revisado. ¡Es el momento de implementarlo! Vamos a dejar que Copilot haga su trabajo mediante Autopilot.

Autopilot permite a Copilot iterar sobre el problema hasta considerar que está completo.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción con una etiqueta similar de la versión instalada.
2. Confirma que el indicador de modo situado debajo de la indicación muestra **Autopilot**.
3. Observa cómo Copilot itera según el plan establecido, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o un conflicto de puertos, resuelve el problema de configuración antes de considerar superadas las comprobaciones.

## Revisar y verificar la implementación

Una vez generado el código, debe revisarse antes de combinarlo, igual que cualquier otro código. Vamos a revisar el código y ejecutar el sitio para comprobar que todo funciona correctamente.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador de modo ya no muestra **Plan** ni **Autopilot**.
2. Introduce `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa la salida de las comprobaciones del proyecto y pide a Copilot que resuelva cualquier error.

## Explorar la nueva funcionalidad

Vale, el código parece correcto, pero ¿funciona? Vamos a iniciar la aplicación como antes y abrir el sitio mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, el filtrado por editor y las combinaciones acordadas en el plan.
4. Confirma que el comportamiento de restablecimiento y de resultados vacíos coincide con la incidencia y tus decisiones.
5. Vuelve al codespace y pide a Copilot que detenga el servidor de desarrollo iniciado.

## Resumen y próximos pasos

Has usado distintos modos de conversación para desarrollar y revisar una funcionalidad. En esta lección:

- has iniciado una conversación nueva de Copilot desde una incidencia de GitHub.
- has definido los requisitos en el modo Plan.
- has implementado la funcionalidad nueva con el modo Autopilot.
- has revisado el código.
- has validado manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizaremos un poco más en cómo se genera el código para comprobar que sigue las prácticas documentadas: [usar instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
