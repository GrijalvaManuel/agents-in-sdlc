---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que documentara el código, creara una función de juegos relacionados para que el equipo de diseño pudiera iterar y añadiera funciones de accesibilidad, incluidos los modos de alto contraste y claro. Revisa los cambios y, si hace falta, proporciona comentarios a Copilot para mejorar el resultado.

### Escenario

Como hemos señalado, los fundamentos del diseño de software y DevOps no cambian al incorporar IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Revisa las sugerencias de Copilot para la documentación, la función de juegos relacionados y las mejoras de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, existen ciertas medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usará para el código.
- El agente en la nube se ejecuta en GitHub Actions, en un entorno independiente y efímero.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que generó el agente en la nube: la que añade documentación al código. Usa la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Puede que veas una advertencia que indica que el firewall ha bloqueado GitHub Copilot. Es **lo esperado**, ya que el acceso del agente a recursos externos, incluidas las llamadas a servidores MCP externos, está limitado de forma predeterminada. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes de incorporación de cambios.
3. Abre la solicitud titulada algo parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot aún está trabajando, la PR tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los comentarios TSDoc y otra documentación. Los cambios exactos pueden variar. Busca los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentarios que proceden de los archivos de instrucciones personalizados del ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Busca un indicador que señale que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot en una PR no es unidireccional. Puedes mencionarlo en comentarios generales o comentarios insertados en el código. Copilot los verá y abrirá otra sesión para responder. Como los resultados no son deterministas, no hay una instrucción exacta que debas usar. Por ejemplo, puedes pedirle:

- que añada encabezados de comentarios a cada archivo de código con una descripción breve.
- que añada comentarios TSDoc a los archivos TypeScript y Astro.
- que cree un README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario en la PR y menciona a **@copilot** como harías con cualquier otra persona. Usa una de las ideas anteriores u otra petición de documentación.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia otra sesión para hacer los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la PR.

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, verás una confirmación nueva en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que el resultado te satisfaga. Después, convierte la PR de borrador en una lista para revisión y combínala.

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Vuelve a la PR que Copilot generó para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada algo parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Busca un indicador que señale que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Comprueba que se superan las comprobaciones de la aplicación Astro; puede tardar unos minutos.
9. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente la función de juegos relacionados. Abre el terminal y ejecuta los siguientes comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, como **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha implementado la función de juegos relacionados. Puedes seguir iterando y pedirle cambios, por ejemplo, modificar el número de juegos relacionados o añadir encabezados y comentarios TSDoc. Recuerda que esta incidencia se asignó **antes** de actualizar las instrucciones personalizadas. Para solicitar cambios, añade un comentario en **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad implementadas con el agente personalizado. Esta PR debería incluir tanto el modo de alto contraste del ejercicio 3 como el modo claro solicitado en el centro de control del ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada algo parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot aún está trabajando, la PR tendrá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** y revisa el código generado.
5. Revisa la implementación y presta atención a:
   - Los componentes de alternancia de la interfaz de usuario.
   - El uso del almacenamiento local para conservar las preferencias.
   - Los cambios de CSS y estilos para los modos de alto contraste y claro.
   - Los atributos de accesibilidad, como las etiquetas ARIA y la navegación con el teclado.
   - El código JavaScript/TypeScript que cambia entre modos.
6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Busca un indicador que señale que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Comprueba que se superan las comprobaciones de la aplicación Astro; puede tardar unos minutos.
10. **Opcional:** cambia a esta rama en tu Codespace para probar las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama que creó Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Comprueba que funcionan y que los ajustes se conservan al volver a cargar la página.

El agente personalizado de accesibilidad ayudó a Copilot a implementar estas funciones siguiendo prácticas recomendadas. Si detectas problemas o mejoras de accesibilidad, menciona a **@copilot** en un comentario y pídele que actualice el código, como hiciste con las otras PR.

## Ejercicio opcional: sigue delegando

El agente en la nube es más útil cuando le asignas tareas reales del backlog y puedes centrarte en otra cosa. Para practicar, crea algunas incidencias para el repositorio y asígnaselas a Copilot. Algunas ideas:

- Crear un formulario de interés para patrocinadores en la página de detalles del juego.
- Implementar la paginación de la lista de juegos.
- Añadir validación de entradas y gestión de errores a los métodos de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** para observar su efecto en el trabajo del agente.
- **Has asignado incidencias al agente en la nube de Copilot** y has visto cómo configura el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y usado un agente personalizado** de accesibilidad para añadir los modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y guiar la sesión de accesibilidad.
- **Has revisado las solicitudes de incorporación de cambios del agente e iterado sobre ellas**, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Repaso y próximos pasos

Has completado el recorrido del agente en la nube. Para seguir explorando, puedes probar los otros recorridos:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración MCP directamente en el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y la opción `/delegate` para volver al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
