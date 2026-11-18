---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas juntas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones, la actualización de la habilidad, el perfil de aseguramiento de la calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya combinaste tú mismo la solicitud de incorporación de cambios (PR) de las valoraciones por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente dividiríamos la funcionalidad, las actualizaciones de las instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para agilizar el taller, hemos mantenido todo el flujo de filtrado y calidad en una conversación y una rama, y todo ese trabajo se incorporará a esta PR.

En esta lección, harás lo siguiente:

- comprender qué es Agent Merge y cómo automatiza el ciclo de vida de la combinación.
- inspeccionar el cambio completo de la funcionalidad y las evidencias de verificación.
- crear la PR del filtrado.
- activar Agent Merge solo después de la revisión y confirmar que la PR se ha combinado.

## Escenario

A lo largo del flujo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR manteniendo bajo control del desarrollador la autorización de la combinación.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para incorporar una solicitud de incorporación de cambios. Cuando lo activas, Copilot resuelve lo que bloquea la PR —corrigiendo comprobaciones fallidas de integración continua (CI), respondiendo a comentarios de revisión y realizando rebase cuando sea necesario— y activa la combinación automática de GitHub cuando el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** tú mismo. Agent Merge puede asumir esa responsabilidad. Revisa el trabajo y decide si está listo antes de activarlo.

## Usar Agent Merge para gestionar la PR

Con todo el código creado, vamos a revisarlo conjuntamente, crear la PR y permitir que Agent Merge gestione el resto del proceso.

1. Vuelve al codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo se complete correctamente, realizará la combinación.
7. Comprueba que aparece un mensaje de Copilot parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite aprobaciones obligatorias, protecciones de rama, colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo informado y completa la combinación revisada manualmente cuando el repositorio lo permita.

## Resumen y próximos pasos

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y ahora el proceso de solicitudes de incorporación de cambios. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de la combinación.
- has inspeccionado el cambio completo de la funcionalidad y las evidencias de verificación.
- has creado la PR del filtrado.
- has activado Agent Merge solo después de la revisión y confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para contexto, modelos, sesiones compartidas y delegación opcional a la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
