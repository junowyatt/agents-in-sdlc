---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados adaptados a tareas o ámbitos concretos dentro del flujo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones centradas, buenas prácticas, patrones de programación y conocimientos específicos del dominio que orienten cómo realiza determinados tipos de trabajo. Los equipos pueden codificar su experiencia en agentes reutilizables: un agente de accesibilidad que cumpla las [WCAG][wcag], un agente de seguridad que siga prácticas de programación segura o un agente de pruebas que mantenga patrones de prueba coherentes.

Los agentes personalizados se definen mediante archivos Markdown en la carpeta `.github/agents` del proyecto o de forma global en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML con, como mínimo, los campos `name` y `description`, seguido de una indicación Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Agentes personalizados frente a habilidades de agente

Existe cierta coincidencia lógica entre los agentes personalizados y las [habilidades de agente][agent-skills-concept]. Ambos se definen principalmente con archivos Markdown e indican a una IA cómo realizar operaciones. La forma más clara de diferenciarlos: un **agente personalizado** es el trabajador y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades —e incluso otros agentes— como parte de su trabajo. En este laboratorio, el agente personalizado de accesibilidad revisa y actualiza el sitio conforme a las directrices de accesibilidad; como parte de ese trabajo podría utilizar habilidades como una habilidad de flujo de solicitudes de incorporación de cambios o una que ejecuta y gestiona pruebas.

> [!NOTE]
> No existe una única manera «correcta» de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba e itera para encontrar el enfoque adecuado para cada entorno y escenario.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a garantizar que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades visuales o preferencias. Los comentarios recientes de usuarios indican que algunas personas encuentran difícil leer el tema oscuro actual debido al contraste insuficiente entre el texto y los colores de fondo. Para solucionar este problema de accesibilidad, el equipo de diseño ha solicitado implementar un modo de alto contraste que las personas puedan activar y desactivar.

Como la accesibilidad es esencial, quieres asegurarte de que se implemente lo antes posible. Utilizarás un agente personalizado para generar la funcionalidad.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente.
- utilizarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado para accesibilidad. Vamos a revisar su contenido para entender cómo orientará a Copilot.

Vuelve al codespace, abre un terminal y cambia a una rama nueva creada a partir de `main` para el trabajo de accesibilidad —mantendrás la PR de filtrado del ejercicio 3 separada—:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. A partir de ahí, revisa las siguientes secciones:
   - Responsabilidades principales al generar código para un sitio web accesible.
   - Buenas prácticas de accesibilidad.
   - Ejemplos de código HTML, CSS y JavaScript.
   - Una lista de errores y problemas habituales.

## Usar el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el selector de agentes situado en la parte inferior de la vista de Copilot Chat. Puedes seleccionar un agente personalizado para limitar una sesión de chat a las instrucciones y herramientas de ese agente.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modelo y el modo varían en cada ejercicio; cada paso lo indicará cuando sea necesario.

1. Selecciona **Agent** en el selector de agentes de la vista de Chat si todavía no está seleccionado.

   ![Captura de pantalla del selector de agentes de la vista de Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el selector de agentes en la parte inferior de la vista de chat —muestra el agente activo; de forma predeterminada, es **default**—.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía la siguiente indicación al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat empezará a trabajar: explorará el código, propondrá cambios y aplicará modificaciones a los archivos del proyecto. Cada modificación aparecerá en línea en el chat con la ruta del archivo y una diferencia que podrás revisar.
6. A medida que se apliquen las modificaciones, el indicador **Files changed** se actualizará para que puedas ver el conjunto de trabajo que el agente ha modificado.

> [!NOTE]
> Este proceso tardará varios minutos. Copilot realiza cambios reales en el repositorio: editará archivos existentes, como los componentes de Astro, los estilos CSS y las pruebas relacionadas.

Revisarás y orientarás este trabajo en curso en el siguiente ejercicio.

## Resumen y próximos pasos

Esta lección ha explorado los [agentes personalizados][custom-agents] de GitHub Copilot: asistentes de IA especializados adaptados a tareas y ámbitos concretos. Con los agentes personalizados puedes codificar la experiencia y los estándares de tu equipo en agentes reutilizables que orientan a Copilot para realizar determinados tipos de trabajo de forma más eficaz.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo usar un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson]: revisarás los cambios a medida que se producen y añadirás un alternador de modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en tu IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Prueba de la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Próxima lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
