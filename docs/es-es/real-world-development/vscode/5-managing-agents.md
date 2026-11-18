---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando activas el modo agente de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo en tiempo real, Copilot Chat en VS Code te ofrece una vista continua de cada llamada a herramientas y edición de archivo a medida que ocurre. Puedes revisar cada diferencia en línea, aceptar o rechazar cambios individuales y orientar la conversación con nuevas indicaciones para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio, harás lo siguiente:

- supervisar el trabajo del agente de accesibilidad en tiempo real.
- revisar y aceptar las modificaciones de archivos propuestas.
- orientar al agente durante la sesión para añadir un alternador de modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras está en curso, el equipo de diseño ha añadido otra solicitud: también quiere un alternador de modo claro para quienes encuentren demasiado oscuro el tema predeterminado. En lugar de empezar una conversación nueva desde cero, orientarás la sesión existente para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista de Copilot Chat muestra cada paso que da el agente: los archivos que explora, los comandos que ejecuta y las modificaciones que propone. Cada modificación de archivo aparece en línea en el chat con una diferencia y controles **Keep** / **Undo**.

1. Vuelve a la vista de Copilot Chat del ejercicio 4.
2. Mientras el agente trabaja, desplázate por el chat para observar cada llamada a herramientas. Verás lecturas de archivos, modificaciones y cualquier comando de terminal que solicite ejecutar.
3. Cuando el agente propone una modificación, la diferencia aparece en línea:
    - Selecciona **Keep** para aceptar el cambio.
    - Selecciona **Undo** para revertir el cambio y permitir que el agente haga otra pasada.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver el conjunto completo de trabajo que el agente ha modificado durante esta sesión.

> [!NOTE]
> Si una llamada a herramientas solicita permiso —por ejemplo, para ejecutar un comando de terminal—, el agente se pausará y mostrará una solicitud **Allow**. Aprueba solo los comandos con los que te sientas cómodo.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios realizados.

## Orientar al agente para añadir el modo claro

Ahora que el modo de alto contraste está preparado, ampliarás la misma conversación para añadir un alternador de modo claro. Como continuarás en la misma sesión de chat, el agente conservará todo el contexto del trabajo anterior, incluidos los componentes que modificó, el enfoque de estilos utilizado y el patrón de almacenamiento local configurado.

1. En el campo de entrada de Copilot Chat, envía la siguiente indicación de seguimiento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde el punto en que lo dejó, explorará los cambios que ya realizó y propondrá las incorporaciones para el modo claro.
3. Revisa cada modificación nueva a medida que aparezca, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** en cada diferencia.
4. Si el agente toma una dirección que no deseas —por ejemplo, intenta refactorizar la interfaz del alternador de una forma que no te guste—, puedes detenerlo con el botón **Stop** situado junto al campo de chat y después enviar un mensaje aclaratorio.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Usa indicaciones de seguimiento para pedir ajustes —«simplifica el CSS: utiliza variables CSS en lugar de duplicar selectores»—, corregir el rumbo —«no añadas un tercer alternador; integra el modo claro en el panel de configuración existente»— o ampliar el alcance —«añade también atajos de teclado para ambos alternadores»—.

## Revisar el conjunto de trabajo

Antes de confirmar el trabajo, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** en VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver actualizaciones en los componentes de Astro, los estilos y las pruebas relacionadas.
3. Abre algunos de los archivos modificados y analiza las diferencias. Confirma que se reflejan los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés satisfecho, añade los cambios al área de preparación y confírmalos desde el panel Source Control. Los publicarás en [la próxima lección][next-lesson].

## Resumen y próximos pasos

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada modificación de archivo a medida que aparecía, aceptado o rechazado cambios y enviado indicaciones de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has explorado:

- supervisar el trabajo de un agente en tiempo real.
- aceptar y rechazar modificaciones individuales de archivos.
- orientar una sesión ya en curso con indicaciones de seguimiento.

En [la próxima lección][next-lesson] publicarás el trabajo de accesibilidad como solicitud de incorporación de cambios y revisarás los cambios producidos por el agente personalizado local a lo largo de este laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Próxima lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
