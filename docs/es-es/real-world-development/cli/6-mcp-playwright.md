---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y úsalo para explorar la funcionalidad de filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos destacado, escribir código no es lo único necesario. Hay que trabajar con datos, servicios externos y automatizaciones adicionales disponibles para Copilot. Aquí es donde entran en juego los servidores MCP. Los servidores de Model Context Protocol (MCP) permiten a Copilot ir más allá de las funciones integradas en la CLI y disponer de más herramientas y servicios.

En esta lección, harás lo siguiente:

- comprender qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- añadir el servidor MCP de Playwright si todavía no está disponible.
- pedir al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Aunque las pruebas unitarias y de extremo a extremo son importantes, validar las actualizaciones de la interfaz requiere interactuar realmente con ella. Quieres permitir que Copilot utilice el sitio web como lo haría una persona usuaria para automatizar aún más cómo se realizan los cambios y aumentar la confianza en que las actualizaciones funcionan como se espera.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] proporciona a los agentes de IA una manera de comunicarse con herramientas y servicios externos. Al utilizar MCP, los agentes pueden comunicarse con ellos en tiempo real. Esto les permite acceder a información actualizada y realizar acciones en tu nombre.

Estas herramientas y recursos se acceden a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. Cada servidor MCP representa un conjunto diferente de herramientas y recursos a los que el agente puede acceder.

Algunos servidores MCP existentes muy conocidos son:

- **[Servidor MCP de GitHub][github-mcp]**: proporciona acceso a las API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Servidor MCP de Playwright][playwright-mcp-server]**: proporciona capacidades de automatización de navegador mediante Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] para mejorar la capacidad de descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica el editor y considera las implicaciones de seguridad.

## Añadir el servidor MCP de Playwright

Si el servidor MCP de Playwright todavía no está disponible, añádelo para que Copilot pueda interactuar con el sitio web como lo haría una persona usuaria.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos de configuración y continúa con la sección siguiente.

1. Vuelve al codespace.
2. Abre el cuadro de diálogo para añadir un servidor MCP introduciendo el siguiente comando en Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. Como nombre, introduce `playwright` y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** pulsando <kbd>Enter</kbd> y después <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el nuevo servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo de MCP.

## Pedir a Copilot que explore la funcionalidad mediante Playwright

Antes habías confirmado manualmente que la funcionalidad se comporta como se esperaba. ¡Ahora vamos a usar el servidor MCP de Playwright disponible en Copilot para hacer lo mismo!

1. Usa la siguiente indicación para pedir a Copilot que utilice el servidor MCP de Playwright y valide la funcionalidad:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No necesitas indicar explícitamente a Copilot que use el servidor MCP, porque normalmente lo detectará por sí solo. Sin embargo, como ya sabes qué debe usar, nunca está de más señalarle la dirección correcta. Ayudará a obtener resultados más constantes y ahorrará algunos tokens.

2. Observa cómo Copilot enumera los distintos pasos que realiza en el navegador para confirmar que la funcionalidad funciona.
3. Lee el informe y comprueba que todo se comporta como se espera.

Copilot iniciará el servidor, utilizará Playwright para interactuar con el sitio web, detendrá el servidor y te dará un informe.

## Resumen y próximos pasos

¡Enhorabuena, has utilizado el servidor MCP de Playwright para explorar tu funcionalidad en un navegador real desde Copilot CLI! En resumen:

- has aprendido qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- has añadido el servidor MCP de Playwright si todavía no estaba disponible.
- has pedido al agente que controle un navegador y explore la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combine la habilidad y las herramientas del navegador en una función especializada.

## Recursos

- [Qué es MCP y por qué se habla tanto de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
