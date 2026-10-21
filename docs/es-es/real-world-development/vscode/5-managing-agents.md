---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

En el modo agente, GitHub Copilot trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code muestra las llamadas a herramientas y las ediciones de archivos a medida que ocurren. Puedes revisar los diff en línea, aceptar o rechazar cambios y guiar la conversación con otras instrucciones, sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones de archivos propuestas.
- guiarás al agente durante la sesión para añadir un control de modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad del [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha solicitado también un modo claro para quienes consideran que el tema predeterminado es demasiado oscuro. En lugar de iniciar otra conversación, guiarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver las llamadas a herramientas, las lecturas y ediciones de archivos y los comandos de terminal que solicita.
3. Cuando el agente proponga una edición, el diff aparecerá en línea:
    - Selecciona **Keep** para aceptar el cambio.
    - Selecciona **Undo** para revertirlo y permitir que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para consultar todos los archivos modificados en la sesión.

> [!NOTE]
> Si una llamada a herramienta solicita permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final con un resumen de los cambios.

## Guía al agente para añadir el modo claro

Ahora que ya está implementado el modo de alto contraste, amplía la misma conversación para añadir un control de modo claro. Como seguirás en la misma sesión de chat, el agente conservará el contexto del trabajo anterior: los componentes que ha modificado, el enfoque de estilos y el patrón de almacenamiento local.

1. En el campo de Copilot Chat, envía esta instrucción:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente revisará sus cambios y propondrá las modificaciones necesarias para añadir el modo claro.
3. Revisa cada cambio nuevo y usa **Keep** o **Undo** en cada diff.
4. Si el agente toma un rumbo que no quieres, como refactorizar los controles de una forma que no te convence, puedes detenerlo con el botón **Stop** junto al campo de chat y enviar otra instrucción para aclarar el resultado esperado.

> [!TIP]
> Guiar una sesión activa es más rápido que empezar de nuevo. Usa instrucciones posteriores para pedir ajustes («simplifica el CSS; usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer control; integra el modo claro en el panel de configuración») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa rápidamente todo lo que haya modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Debería incluir componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa los diff. Confirma que siguen las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación por teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y próximos pasos

Has usado el modo agente de Copilot Chat para supervisar y guiar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado instrucciones posteriores para ampliar el trabajo sin perder contexto.

En esta lección:

- has supervisado el trabajo del agente en tiempo real.
- has aceptado y rechazado ediciones de archivos.
- has guiado la sesión con instrucciones posteriores.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que ha generado el agente personalizado.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
