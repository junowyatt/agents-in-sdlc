---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Próxima lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios, debes confirmar que realmente funciona en el navegador. En lugar de recorrer tú la aplicación, conectarás el **servidor MCP de Playwright** y dejarás que Copilot controle un navegador real para probar la funcionalidad; después publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas.
- añadirás el servidor MCP de Playwright al espacio de trabajo.
- pedirás a Copilot que lo utilice para probar manualmente la funcionalidad de filtrado en un navegador.
- publicarás la rama y abrirás una solicitud de incorporación de cambios con el trabajo de filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente adquiere mucha más potencia cuando puede acceder a recursos externos al editor. Model Context Protocol (MCP) es el mecanismo que permite a Copilot hacerlo: se trata de un estándar para que el agente se comunique con herramientas y servicios externos.

![Diagrama que muestra el funcionamiento interno del modo agente y cómo interactúa con el contexto, el modelo de lenguaje grande y las herramientas, incluidas las herramientas aportadas por servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) proporciona a los agentes de IA una manera de comunicarse con herramientas y servicios externos. Al utilizar MCP, los agentes pueden comunicarse con ellos en tiempo real. Así pueden acceder a información actualizada —mediante recursos— y realizar acciones en tu nombre —mediante herramientas—.

Estas herramientas y recursos se acceden a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. El servidor MCP gestiona la comunicación entre el agente de IA y las herramientas externas —como API existentes o herramientas locales como paquetes de npm—. Cada servidor MCP representa un conjunto diferente de herramientas y recursos a los que el agente puede acceder.

Algunos servidores MCP conocidos son:

- **[Servidor MCP de GitHub](https://github.com/github/github-mcp-server)**: proporciona acceso a un conjunto de API para gestionar repositorios de GitHub. Permite al agente crear repositorios, actualizar los existentes y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Servidor MCP de Playwright](https://github.com/microsoft/playwright-mcp)**: proporciona capacidades de automatización de navegador mediante Playwright. Permite al agente navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP disponibles que permiten acceder a distintas herramientas y recursos. GitHub aloja un [registro de MCP](https://github.com/mcp) para mejorar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Desde el punto de vista de la seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa cuidadosamente su código fuente, verifica el editor y considera las implicaciones de seguridad. Utiliza únicamente servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el codespace.
2. Deberías ver un servidor `github` ya configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Esta entrada `github` se incluye con la plantilla del proyecto, por lo que Copilot pudo leer tu backlog de incidencias en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hay que instalar nada localmente: VS Code se conecta mediante HTTP y la primera vez que Copilot utilice una de sus herramientas deberás autenticarte con GitHub.

## Añadir el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, exactamente lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo tenga este aspecto:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia en tu equipo ejecutando `npx @playwright/mcp@latest`. El marcador `--headless` indica a Playwright que ejecute el navegador sin una ventana visible, lo que es necesario dentro de un codespace que no tiene escritorio.

> [!NOTE]
> El proyecto Tailspin Toys ya utiliza Playwright para sus pruebas de extremo a extremo, así que el navegador que necesita Playwright debería estar instalado. Si Copilot informa después de que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Iniciar y confiar en el servidor

VS Code inicia los servidores MCP **bajo demanda**: la primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, aparece una acción **Start** sobre la entrada `playwright`. Selecciónala para iniciar el servidor ahora y confirmar que la configuración es correcta.
2. Si VS Code te pide que confirmes que confías en el servidor, revisa la configuración y elige confiar para que pueda iniciarse.

Iniciarlo manualmente es opcional: si lo omites, VS Code iniciará el servidor —y te pedirá que confíes en él— la primera vez que Copilot necesite sus herramientas en el siguiente paso.

## Probar la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real que puede controlar. En lugar de recorrer tú la aplicación para comprobar el trabajo, el agente puede abrir una página, navegar, aplicar filtros y comunicarte los resultados; después resumirá lo observado. Es la forma más rápida de confirmar que una funcionalidad se comporta como esperas sin salir de la conversación.

Internamente, el servidor MCP de Playwright trabaja a partir del [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de utilizar capturas de pantalla. Esto significa que el agente razona sobre elementos estructurados y etiquetados —botones, enlaces y elementos de lista— del mismo modo que lo haría una tecnología de asistencia. Así, una comprobación funcional rápida también sirve como revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la funcionalidad de filtrado que acabas de crear:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot iniciará un navegador mediante el servidor MCP de Playwright, seguirá cada paso y te comunicará lo que encuentre. Compara el resumen con los criterios de aceptación de la incidencia: si detectas algún problema, formula preguntas de seguimiento o haz que Copilot corrija el código antes de abrir una solicitud de incorporación de cambios.

> [!NOTE]
> La aplicación debe estar ejecutándose en `http://localhost:4321` para esta prueba. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar la indicación. La primera vez que Copilot utilice el servidor MCP de Playwright puede tener que descargar un navegador; si informa de que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una solicitud de incorporación de cambios

Ahora que has confirmado que la funcionalidad funciona, puedes abrir una solicitud de incorporación de cambios (PR) para que el equipo la revise. El primer paso es publicar la rama `filtering-vscode`.

1. Navega al panel **Source Control** del codespace y revisa los cambios realizados por Copilot.
2. Añade los cambios al área de preparación seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura de pantalla del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crear la solicitud de incorporación de cambios

Hay varias maneras de crear una solicitud de incorporación de cambios, entre ellas desde github.com y desde la interfaz de línea de comandos (CLI) de GitHub. Como ya estás trabajando con GitHub Copilot, ¡dejemos que cree la PR por ti! Puede encontrar la incidencia relevante y crear la PR asociada.

1. Navega al panel de Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Comprueba que **Agent** está seleccionado en el selector de agentes para que Copilot pueda utilizar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Si se solicita, selecciona **Continue** para permitir que Copilot realice las tareas necesarias para reunir información y ejecutar operaciones. La primera vez que utilice una herramienta de GitHub puede pedirte que inicies sesión en GitHub; sigue las indicaciones para permitirlo.
5. Observa cómo Copilot busca en las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace generado por Copilot para revisar la solicitud de incorporación de cambios, pero **todavía no la combines**.

## Resumen y próximos pasos

¡Enhorabuena! En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas.
- has añadido el servidor MCP de Playwright al espacio de trabajo.
- lo has utilizado para probar manualmente la funcionalidad de filtrado en un navegador antes de publicar el trabajo.
- has publicado la rama y abierto una solicitud de incorporación de cambios con el trabajo de filtrado.

Has utilizado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una manera de ampliar Copilot. A continuación, [crearemos un agente personalizado][next-lesson] para agilizar tareas centradas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué se habla tanto de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
