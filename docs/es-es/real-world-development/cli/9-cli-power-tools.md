---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar opcionalmente a un agente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra para interactuar con ella. Estos comandos exponen funciones avanzadas, información interna y opciones de configuración adicionales. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Vamos a explorar algunos otros útiles.

En esta lección, harás lo siguiente:

- usar `/context` y `/compact` para explorar cómo Copilot gestiona el contexto de la conversación.
- usar `/model` para explorar los modelos disponibles.
- aprender cómo `/share` puede exportar o compartir una sesión.
- explorar comandos opcionales para trabajo paralelo, árboles de trabajo y delegación al agente en la nube.

## Escenario

Ya has completado el flujo principal de la CLI. Ahora vamos a revisar otras capacidades: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar opcionalmente el trabajo a [Copilot cloud agent][about-cloud-agent].

## Explorar el contexto de Copilot CLI

Cuando trabajas en tareas mayores o más complejas, puedes alcanzar el límite de la ventana de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando hace falta y puedes inspeccionar o compactar el contexto tú mismo con comandos de barra.

1. Vuelve al codespace e inicia Copilot CLI desde la raíz del repositorio si todavía no está abierto.
2. Introduce:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y cómo se divide el contexto entre instrucciones del sistema, herramientas, mensajes y espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Introduce `/context` de nuevo y compara el resultado. Puede que no haya cambios grandes si la conversación ya era pequeña.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto a medida que se llena la ventana. Usa `/compact` si quieres elegir el momento. Usa `/clear` o `/new` al cambiar a una tarea diferente y quieres una conversación nueva.

## Elegir el modelo

Los distintos modelos tienen diferentes puntos fuertes y distintos desarrolladores tienen preferencias distintas. Copilot CLI permite listar y seleccionar el modelo que quieras usar.

1. Introduce:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Trabajar en equipo y compartir aprendizajes ayuda a todos a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace compartible o publicar un gist de GitHub.

1. Introduce `/help` y revisa las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir esta sesión, elige el destino adecuado, por ejemplo `/share file` para una exportación Markdown local.
3. Revisa el contenido exportado antes de enviarlo a alguien o publicarlo. Las exportaciones de sesiones pueden incluir indicaciones, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio ni de la conversación que tu equipo no tenga intención de compartir.

## Opcional: ampliar o delegar

El taller principal está completo. Copilot CLI también ofrece comandos para tareas mayores:

- `/fleet` puede dividir subtareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo Git aislado para una tarea independiente.
- `/delegate` puede enviar una tarea a Copilot cloud agent, que trabaja de forma asincrónica y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear otros árboles de trabajo o trabajo remoto. Antes de probarlos, inicia una tarea nueva y bien definida, y revisa el resultado siguiendo tu flujo normal. Si quieres profundizar en el trabajo asincrónico con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y próximos pasos

El uso de comandos de barra en Copilot CLI permite configurar la herramienta, compartir sesiones y ver qué ocurre entre bastidores. En esta lección:

- has usado `/context` y `/compact` para explorar cómo Copilot gestiona el contexto de la conversación.
- has usado `/model` para explorar los modelos disponibles.
- has aprendido cómo `/share` puede exportar o compartir una sesión.
- has explorado comandos opcionales para trabajo paralelo, árboles de trabajo y delegación al agente en la nube.

Hay más comandos de barra y mucho más por explorar con Copilot CLI. Vamos a cerrar el recorrido [revisando lo aprendido][next-lesson] y los próximos pasos para seguir aprendiendo.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca de Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
