---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

A lo largo de este laboratorio has trabajado con GitHub Copilot en varias tareas centradas en mejorar la experiencia de usuario y añadir funcionalidades. Has pedido a Copilot que añada documentación al código, que cree una funcionalidad de juegos relacionados para que el equipo de diseño pueda iterar sobre ella y que implemente funciones de accesibilidad, incluidos los modos de alto contraste y claro. Ahora vamos a explorar los cambios de código y, si hace falta, proporcionar comentarios a Copilot para mejorar su trabajo.

### Escenario

Como hemos destacado muchas veces, los fundamentos del diseño de software y de DevOps no cambian con la incorporación de IA generativa. Siempre conviene revisar el código generado y seguir el proceso habitual de DevOps. Teniendo esto en cuenta, vamos a revisar las propuestas de GitHub Copilot para crear la documentación, la funcionalidad de juegos relacionados y las funciones de accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y el agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot realiza las tareas de forma asincrónica y sin supervisión, se han establecido determinadas restricciones de seguridad para garantizar que todo permanezca protegido. Estas incluyen:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura únicamente a la rama que utilizará para el código.
- El agente en la nube se ejecuta dentro de GitHub Actions, donde crea un entorno separado y efímero en el que trabajar.
- Cualquier flujo de trabajo de GitHub Actions requiere la aprobación de una persona antes de ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Vamos a empezar explorando la primera solicitud de incorporación de cambios (PR) generada por el agente en la nube de GitHub Copilot: la que añade documentación al código. Realizarás esta tarea mediante la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Cuando explores la PR puedes ver una advertencia que indica que GitHub Copilot ha sido bloqueado por un firewall. Esto es **esperado**, ya que Copilot tiene acceso limitado a recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si lo deseas, puedes [personalizar o desactivar el firewall del agente en la nube de Copilot][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes de incorporación de cambios.
3. Abre la solicitud titulada de forma parecida a **Add missing documentation** o con un título más específico.

> [!NOTE]
> Si Copilot todavía está trabajando en la tarea, la solicitud contendrá el indicador **[WIP]**. En ese caso, espera a que Copilot termine el trabajo. Puede tardar unos minutos, así que puedes hacer una pausa o reflexionar sobre todo lo aprendido hasta ahora.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios de documentación TSDoc y otra documentación. Los cambios exactos pueden variar.

    Mientras revisas los cambios, busca comentarios de documentación TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones personalizados que revisaste al inicio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que muestra que algunos flujos de trabajo están esperando aprobación.
8. Si los flujos de trabajo esperan aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían ponerse en cola en la sección de comprobaciones de la solicitud de incorporación de cambios. Si todo va bien, las comprobaciones del proyecto deberían superar las pruebas de la aplicación Astro única. Este proceso puede tardar unos minutos.

## Solicitar cambios a GitHub Copilot

Trabajar con Copilot en una solicitud de incorporación de cambios no es un proceso unidireccional. También puedes etiquetar a Copilot en comentarios —como harías con cualquier miembro del equipo— dentro de la PR o en líneas concretas del código. Copilot verá esos comentarios y iniciará otra sesión para abordarlos. Debido a los resultados no deterministas, no podemos darte un texto prescriptivo exacto. Algunas ideas de cambios que puedes pedir a Copilot:

- Añadir encabezados de comentario al principio de cada archivo de código con una breve descripción de su función.
- Añadir comentarios de documentación TSDoc a los archivos de TypeScript y Astro.
- Crear un README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario solicitando un cambio en la documentación generada y etiqueta a **@copilot**, como harías con cualquier usuario. Utiliza una de las ideas anteriores u otra sugerencia relacionada con la documentación que quieras ver en el código base.
2. Selecciona **View Session** para observar cómo Copilot realiza el trabajo. Verás que inicia una sesión nueva para aplicar las actualizaciones.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot haya completado los cambios, verás un commit nuevo en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés satisfecho. Cuando lo estés, convierte la PR de borrador a lista para revisión y combínala en la rama principal.

![Convertir la PR a lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Vamos a volver a la PR generada por Copilot para resolver la incidencia sobre cómo mostrar juegos relacionados en la página de detalles del juego.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page** o a una descripción más detallada.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que muestra que algunos flujos de trabajo están esperando aprobación.
7. Si los flujos de trabajo esperan aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían ponerse en cola en la sección de comprobaciones de la solicitud de incorporación de cambios. Si todo va bien, las comprobaciones del proyecto deberían superar las pruebas de la aplicación Astro única. Este proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en el codespace para realizar una prueba manual de la funcionalidad de juegos relacionados. Navega al codespace, abre el terminal y ejecuta los siguientes comandos —sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo **copilot/fix-8**—:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

¡Copilot ha creado la funcionalidad de juegos relacionados! Como antes, puedes trabajar de forma iterativa con el agente en la nube de Copilot para solicitar actualizaciones. Por ejemplo, puedes pedir que se modifique cuántos juegos relacionados se muestran o que se añadan encabezados de comentario y comentarios de documentación TSDoc —recuerda que esta tarea se asignó antes de actualizar las instrucciones personalizadas—. Como antes, puedes realizar estas solicitudes añadiendo un nuevo comentario en la pestaña **Conversation**, que Copilot verá y con el que iniciará una sesión nueva.

## Revisar las funciones de accesibilidad

Por último, vamos a revisar las funciones de accesibilidad implementadas mediante el agente personalizado de accesibilidad. Esta PR debería incluir tanto el modo de alto contraste asignado en el ejercicio 3 como el modo claro solicitado desde Mission Control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website** o a una descripción más detallada.

> [!NOTE]
> Si Copilot todavía está trabajando en la tarea, la solicitud contendrá el indicador **[WIP]**. En ese caso, espera a que Copilot termine el trabajo.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - Los componentes de interfaz que permiten alternar entre modos.
   - El uso del almacenamiento local para conservar las preferencias del usuario.
   - Los cambios de CSS o de estilos para los modos de alto contraste y claro.
   - Los atributos de accesibilidad —etiquetas ARIA, navegación por teclado, etc.—.
   - El código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que muestra que algunos flujos de trabajo están esperando aprobación.
8. Si los flujos de trabajo esperan aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían ponerse en cola en la sección de comprobaciones de la solicitud de incorporación de cambios. Si todo va bien, las comprobaciones del proyecto deberían superar las pruebas de la aplicación Astro única. Este proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en el codespace para probar manualmente las funciones de accesibilidad. Navega al codespace, abre el terminal y ejecuta los siguientes comandos —sustituye `<branch-name>` por el nombre de la rama creada por Copilot—:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después inicia la aplicación y prueba los controles de modo de alto contraste y modo claro en el navegador para comprobar que funcionan como se espera y mantienen el estado tras recargar la página.

Observa cómo el agente personalizado de accesibilidad ayudó a orientar a Copilot para implementar estas funciones siguiendo las buenas prácticas de accesibilidad. Si encuentras alguna preocupación o mejora relacionada con la accesibilidad, puedes etiquetar a **@copilot** en un comentario para solicitar actualizaciones, como hiciste con las PR anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube funciona mejor cuando puedes entregarle elementos reales del backlog y centrarte en otras tareas. Para adquirir ese hábito, crea algunas incidencias más en el repositorio y asígnalas a Copilot. Algunas ideas:

- Crear un formulario de interés para patrocinadores en la página de detalles del juego.
- Implementar paginación en la lista de juegos.
- Añadir validación de entradas y gestión de errores a las funciones auxiliares de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. A lo largo de estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados incluidos en este repositorio** para observar después su efecto en el resultado del agente en la nube.
- **Has asignado incidencias al agente en la nube de Copilot** y observado cómo configura el entorno, planifica y ejecuta el trabajo de forma asincrónica.
- **Has creado y usado un agente personalizado** para accesibilidad, añadiendo controles de modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como Mission Control** para supervisar y dirigir la sesión de accesibilidad mientras estaba en curso.
- **Has revisado e iterado sobre las solicitudes de incorporación de cambios del agente en la nube**, etiquetando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Revisión y próximos pasos

Has completado el entorno del agente en la nube. Si quieres seguir explorando, los otros entornos complementan lo que has practicado aquí:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración MCP directamente desde tu IDE.
- 💻 **[Entorno de CLI](../../cli/)**: realiza dos cambios revisados desde el terminal con Copilot CLI, modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y un puente opcional `/delegate` hacia el agente en la nube.

En tu propio repositorio, prueba estas ideas de seguimiento:

- Asignar una incidencia de refactorización o cobertura de pruebas al agente en la nube.
- Crear un agente personalizado para otro ámbito.
- Configurar `copilot-setup-steps.yml` para una pila tecnológica diferente.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas de configuración del agente en la nube de Copilot][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
