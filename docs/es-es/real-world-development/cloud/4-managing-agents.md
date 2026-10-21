---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que realice tres tareas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes consultar las tareas que tienen solicitudes de incorporación de cambios abiertas, explorar las operaciones realizadas y guiar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- guiarás una sesión activa para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo decidió que también sería buen momento para añadir un modo claro. Como ya se está actualizando el estilo del sitio y se están añadiendo controles para cambiarlo, parece lógico incluir esta función. Guiarás al agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Consulta el estado de todas las tareas asignadas al agente en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en los paneles principal e izquierdo. Deberías ver las tareas asignadas a Copilot, como:
    - Actualizar la documentación del código.
    - Generar API para modificar juegos.
    - Añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso. Revisa las operaciones realizadas por Copilot, que pueden incluir:
    - Obtener el código del repositorio.
    - Crear el entorno de trabajo.
    - Configurar servidores MCP.
    - Realizar los distintos pasos para completar la tarea.

> [!NOTE]
> Los pasos que se muestran dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de la solicitud de incorporación de cambios (PR), que aparece a la derecha. Permite consultar la PR y los archivos modificados.

## Guía al agente en la nube

Ahora que ya has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto depende del nombre elegido por Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos a que la sesión termine la configuración y empiece a trabajar. Lo sabrás cuando aparezcan mensajes como los del ejemplo.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe esta instrucción:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro Steer active session while Copilot is working resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la instrucción.
5. Observa cómo Copilot confirma la solicitud y la incorpora a su trabajo.

## Deja que Copilot trabaje

Copilot retomará la tarea actualizada e incorporará la nueva solicitud cuando termine el paso en curso.

El proceso puede tardar varios minutos. Mientras tanto, puedes hacer una pausa y repasar lo aprendido.

## Resumen y próximos pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y guiar las tareas del agente en la nube de GitHub Copilot. Puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir una sesión para cambiar su alcance o proporcionar más indicaciones.

Has explorado estos conceptos:

- usar el centro de control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigir una sesión en curso para pedir una funcionalidad adicional.

Mientras Copilot termina las funciones de accesibilidad, puedes [iterar sobre sus solicitudes de incorporación de cambios][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
