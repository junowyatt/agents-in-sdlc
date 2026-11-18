---
title: "Lección 10 - Resumen y próximos pasos"
description: "Recapitula el flujo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI a lo largo de un flujo continuo con Tailspin Toys. En concreto:

- has preparado un codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado inicial.
- has añadido valoraciones por estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR).
- has empezado desde la incidencia de filtrado, definido el enfoque en el modo Plan, desarrollado la funcionalidad en el modo Autopilot y revisado el resultado en el modo Interactive.
- has orientado al agente con instrucciones personalizadas y después personalizado la habilidad existente `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has utilizado para explorar el filtrado en un navegador real.
- has creado y seleccionado un agente personalizado de aseguramiento de la calidad (QA) para evaluar requisitos, cobertura, resultados de habilidades y evidencias del navegador.
- has revisado el cambio completo de filtrado y autorizado Agent Merge para la PR del filtrado.
- has explorado comandos de barra para contexto, modelos, sesiones compartidas y delegación opcional a la nube.

## Qué has publicado

El taller tiene dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizado:

1. **Valoraciones por estrellas:** muestra la propiedad `starRating` existente y un estado explícito de sin valorar en las tarjetas de juegos.
2. **Flujo de filtrado y calidad:** implementa el filtrado, actualiza las instrucciones y aplícalas a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas asociadas.

Desde la planificación del filtrado hasta la apertura de su PR, has utilizado la misma conversación y rama. Combinamos ese trabajo en una PR para agilizar el taller.

## Diferentes tipos de verificación

Has comprobado el código de varias formas: pruebas automatizadas, revisión propia en el navegador y exploración del navegador realizada por Copilot mediante MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos y mostró los resultados en el nuevo formato. QA reunió esos resultados con una revisión de requisitos y cobertura de pruebas antes de crear la PR.

Las pruebas añadidas deben cerrar lagunas reales; una ejecución de QA que no necesite nuevas pruebas puede ser correcta. Revisa el código y las evidencias antes de autorizar la combinación y actualiza las evidencias afectadas después de realizar cambios.

## Buenas prácticas

El contexto y las herramientas que proporcionas a Copilot determinan su trabajo. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a las necesidades de tu equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen funciones especialistas y los servidores MCP conectan herramientas externas. Revisa los cambios reales y los resultados de las herramientas, no solo el resumen del agente.

Haz coincidir el **modo y el modelo** con la tarea. Usa **Plan** para definir un enfoque antes de desarrollar, **Interactive** para mantenerte al tanto en cambios centrados y **Autopilot** en tareas bien definidas. Elige un modelo rápido para cambios rutinarios y uno más avanzado para trabajos complejos.

El contexto sigue siendo tan importante como la infraestructura. Describir claramente qué quieres construir, por qué y cómo cambia de forma significativa el resultado.

## Más por explorar

Has cubierto el flujo principal. Algunas funciones más de la CLI que merece la pena conocer:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para hablar de un problema y obtener otra perspectiva.
- `/fleet` para coordinar subtareas independientes en paralelo.
- `/worktree` para aislar una tarea independiente.
- `/delegate` para enviar una tarea a Copilot cloud agent.

## Próximos pasos

¡La mejor manera de mejorar con cualquier herramienta es seguir utilizándola! Úsala para código de producción, proyectos personales o esa pequeña aplicación que llevas años teniendo en mente sin llegar a crear. Comparte tus aprendizajes con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller de Copilot cloud agent][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
