---
title: "Lección 7 - Crear y usar un agente de QA"
description: "Crea un agente personalizado de QA que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has utilizado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar la experiencia de filtrado en un navegador. Ahora reunirás esas capacidades en un agente personalizado con un proceso de QA claramente definido.

En esta lección, harás lo siguiente:

- explorar cómo un agente personalizado trabaja con instrucciones, habilidades y herramientas MCP.
- crear e inspeccionar un perfil reutilizable de aseguramiento de la calidad.
- seleccionar el agente de QA y revisar sus resultados frente a la incidencia de filtrado.

## Escenario

Tailspin Toys quiere una revisión coherente de los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador antes de abrir una solicitud de incorporación de cambios (PR). Un agente personalizado puede coordinar ese proceso de QA y proporcionar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe el propósito del agente, sus instrucciones y las herramientas disponibles. Para este taller, definirás una función de QA en `.github/agents/qa.agent.md` y la seleccionarás en Copilot CLI.

Las personalizaciones que has usado tienen trabajos diferentes. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` reúne comprobaciones repetibles. Playwright MCP proporciona herramientas de navegador. El perfil de QA indica a Copilot cómo utilizar esas capacidades para evaluar requisitos e informar de los resultados. No reemplaza esas funciones ni requiere otra conversación.

## Crear el perfil de QA

Antes de abrir la PR de la funcionalidad, pedirás a Copilot que cree un perfil reutilizable de QA. El perfil definirá tanto las comprobaciones que realiza QA como los límites que debe respetar.

1. Vuelve al codespace y confirma que la conversación de filtrado está en el modo Interactive.
2. Pide al agente predeterminado que cree el nuevo agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspeccionar el perfil

Antes de usar el nuevo agente, revisa su perfil para confirmar que Copilot ha recogido el flujo de QA y sus límites. Esto evita que un agente incompleto o demasiado amplio modifique la funcionalidad cuando solo quieres verificarla.

1. Introduce `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. El campo `description` es obligatorio; `name` es opcional, pero incluirlo proporciona al agente un nombre claro para mostrar.
3. Lee las instrucciones del perfil y confirma que QA empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y utiliza Playwright MCP.
4. Confirma que QA aporta evidencias de apoyo, pide permiso antes de cambiar código de implementación y no confirma cambios ni abre solicitudes de incorporación de cambios.
5. Si el perfil generado omite alguna responsabilidad o límite, pide al agente predeterminado que lo revise antes de continuar.

## Ejecutar QA frente a la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Reanuda la misma conversación de filtrado después de crear el perfil y selecciona QA para que pueda utilizar la incidencia y las decisiones de planificación ya disponibles en el contexto.

1. Activa el agente con la siguiente indicación:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, es posible que no aparezca en la lista de agentes disponibles. Sin embargo, está creado y el comando anterior lo activará.

2. Usa la siguiente indicación para pedir al agente de QA que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. ¡El agente de QA se pone a trabajar!
4. Lee el informe que proporciona cuando termina su trabajo.

## Resumen y próximos pasos

Has añadido una función especializada reutilizable al flujo de trabajo y revisado su trabajo. En esta lección:

- has explorado cómo un agente personalizado trabaja con instrucciones, habilidades y herramientas MCP.
- has creado e inspeccionado un perfil reutilizable de aseguramiento de la calidad.
- has seleccionado el agente de QA y revisado sus resultados frente a la incidencia de filtrado.

Ya tienes preparada la implementación, la actualización de la habilidad, el perfil de QA, las pruebas y el informe de verificación para la revisión. A continuación, [los reunirás en una PR de funcionalidad y utilizarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
