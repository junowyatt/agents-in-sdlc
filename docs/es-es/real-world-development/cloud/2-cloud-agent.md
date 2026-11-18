---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Probablemente haya muy pocas organizaciones, si es que existe alguna, que no tengan problemas con la deuda técnica. Puede tratarse de incidencias de seguridad sin resolver, código heredado que necesita actualizaciones o solicitudes de funcionalidad que llevan mucho tiempo en el backlog porque nunca se dispone de tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando el agente termina su trabajo, genera una PR en borrador lista para que un desarrollador humano la revise. Esto permite delegar tareas tediosas y acelerar el proceso de desarrollo, además de liberar a los desarrolladores para que se centren en cuestiones de mayor alcance.

Explorarás lo siguiente con el agente en la nube de Copilot:

- personalizar el entorno para generar código.
- garantizar que las operaciones se realizan de forma segura.
- la importancia de definir correctamente el alcance de las incidencias.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere resolver parte de su deuda técnica. Los contratistas que crearon inicialmente la primera versión del sitio dejaron la documentación en un estado poco conveniente —y, de hecho, era completamente inexistente—. Como primer paso, quieren que se añadan comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño está preparado para mejorar el descubrimiento de juegos. Quiere que la página de detalles de cada juego muestre juegos relacionados: otros títulos de la misma categoría, para que la gente pueda seguir navegando. Todavía no necesitan una implementación pulida, solo algo que puedan utilizar para realizar pruebas de aceptación de la experiencia de usuario. Esta cuestión bloquea el trabajo, pero hay otras incidencias de mayor prioridad.

Ambas son tareas que pueden quedar relegadas rápidamente y son adecuadas para asignarlas al agente en la nube de Copilot. Así podrá trabajar de forma asincrónica, permitiendo al desarrollador centrarse en otras tareas y volver después para revisar el trabajo de Copilot y comprobar que todo es correcto.

## Presentación del agente en la nube de GitHub Copilot

[El agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano, de manera similar a una persona desarrolladora. Y, como al trabajar con una persona, se puede hacer de varias formas, entre ellas [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot crea una solicitud de incorporación de cambios en borrador para seguir el progreso, prepara un entorno y empieza a trabajar en la tarea. Puedes consultar la sesión de Copilot mientras está en curso o después de finalizar. Cuando la solución propuesta esté lista para revisión, Copilot te etiquetará en la solicitud de incorporación de cambios.

## La importancia de unas instrucciones bien definidas

GitHub Copilot no hace magia: no elimina la necesidad de pensar. Incluso operaciones aparentemente sencillas adquieren complejidad al profundizar en ellas, así que [define con cuidado el alcance de las tareas del agente en la nube][cloud-agent-best-practices]. Trátalo como a una pareja de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian con la incorporación de IA generativa.

## Instrucciones personalizadas de este repositorio

Los ejercicios anteriores introdujeron las instrucciones personalizadas y la forma en que orientan a Copilot. Si ya has completado la práctica de instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en funcionamiento; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube de Copilot, consulta los archivos de instrucciones que ya incluye el repositorio para detectar después su efecto.

Abre los siguientes archivos en la interfaz web de GitHub de tu repositorio o en un codespace si ya tienes uno en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las expectativas sobre las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que limita estas instrucciones a archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` a Copilot en la siguiente sección, observa en la solicitud de incorporación de cambios los comentarios de documentación TSDoc, las convenciones de TypeScript y los encabezados de comentario que se apliquen. Estos proceden de los archivos de instrucciones. Lo volveremos a señalar al revisar las PR en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente en la nube de Copilot

Crear código, independientemente de quién participe, suele requerir un entorno específico y scripts de configuración que hay que ejecutar para dejarlo en buen estado. Esto también es válido al asignar tareas a Copilot, que las realiza de forma similar a una persona desarrolladora de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizar ese entorno creando un [flujo de trabajo de configuración especial][setup-workflow], configurado en `.github/workflows/copilot-setup-steps.yml`, para ejecutarlo antes de que Copilot empiece a trabajar. Así obtiene acceso a las herramientas y dependencias de desarrollo necesarias. Este flujo ya está configurado para el laboratorio, lo que permite que el flujo siga adelante y ofrece esta oportunidad de aprendizaje. Se asegura de que Copilot tenga acceso a Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y poblada de la aplicación Astro única:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en esos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (ahora toda la aplicación usa Astro y Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos iniciales en SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

Tiene el aspecto de cualquier otro archivo de flujo de trabajo, pero tiene algunos puntos clave:

- Contiene un único trabajo denominado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Observa que el desencadenador `workflow_dispatch` permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions**. Esto resulta útil para probar el flujo en lugar de esperar a que lo ejecute Copilot.

## Añadir documentación

Aunque todo el mundo entiende la importancia de la documentación, la mayoría de los proyectos tienen información obsoleta o carecen de ella por completo. Esta es la clase de deuda técnica que muchas veces no se aborda, reduce la productividad y dificulta el mantenimiento del código base o la incorporación de nuevos desarrolladores al equipo. Por suerte, Copilot destaca en la creación de documentación, y esta es una incidencia perfecta para asignarla al agente en la nube de Copilot. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior volverás a revisar su trabajo.

1. Navega a tu repositorio en github.com desde una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para crear la nueva incidencia.
5. Establece el **Title** como `Code lacks documentation`.
6. Establece la **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En la parte derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de la asignación a Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios recién generada, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si no aparece una PR nueva en la lista, espera un momento y actualiza la ventana del navegador.
12. Después de unos minutos, Copilot habrá creado una lista de tareas pendientes.

> [!NOTE]
> Puede pasar varios minutos hasta que aparezca la lista de tareas de Copilot en la PR. Copilot está creando el entorno (ejecutando el flujo de trabajo indicado antes), analizando el proyecto y determinando el mejor enfoque para abordar el problema.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la solicitud de incorporación de cambios y verás una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes desplazarte por la sesión en directo y ver cómo Copilot resuelve el problema: explora el código, comprende su estado, pausa para pensar y decidir el plan adecuado y, finalmente, crea código.

Probablemente tardará varios minutos. Uno de los objetivos principales del agente en la nube de Copilot es permitirle realizar tareas de forma asincrónica y liberarte para que puedas centrarte en otras tareas. Vamos a aprovechar precisamente esa función asignando otra tarea al agente en la nube y dirigiendo después nuestra atención a escribir código para añadir funciones a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Como hemos destacado, una de las grandes ventajas del agente en la nube de Copilot es la capacidad de dividir el trabajo: puedes centrarte en un conjunto de tareas mientras él se ocupa de otro. Aunque añadir una sección de juegos relacionados para el equipo de diseño podría no llevar mucho tiempo, es tiempo que podría utilizarse en otras tareas. ¡Vamos a asignársela al agente en la nube de Copilot!

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para utilizar la plantilla en blanco.
5. Establece el **Title** como `Show related games on the game details page`.
6. Establece la **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. En la parte derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, deberías ver un conjunto de 👀 en el primer comentario de la incidencia, lo que indica que Copilot está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

¡Copilot está trabajando diligentemente en tu segunda solicitud! El agente en la nube de Copilot funciona de manera similar a una persona desarrolladora de software, así que no necesitas supervisarlo activamente; basta con revisar el resultado cuando haya terminado. Vamos a dirigir la atención a crear y utilizar agentes personalizados.

## Resumen y próximos pasos

Esta lección ha explorado [el agente en la nube de GitHub Copilot][copilot-agents]. Con el agente en la nube puedes asignar incidencias a Copilot para que las resuelva de forma asincrónica. Puedes utilizar Copilot para resolver deuda técnica, crear funcionalidades nuevas o migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno para generar código.
- garantizar que las operaciones se realizan de forma segura.
- la importancia de definir correctamente el alcance de las incidencias.
- asignar incidencias a Copilot.

Con el agente en la nube trabajando diligentemente en segundo plano, ahora podemos dirigir la atención a crear y utilizar agentes personalizados. [El agente en la nube de Copilot también puede utilizar servidores MCP][cloud-agent-mcp] y tiene disponibles las instrucciones personalizadas que hemos explorado en ejercicios anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas de configuración del agente en la nube de Copilot][cloud-agent-best-practices]

| [Próxima lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
