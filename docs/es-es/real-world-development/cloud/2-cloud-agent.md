---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que muchas organizaciones tengan problemas de deuda técnica: incidencias de seguridad sin resolver, código heredado que necesita actualizarse o funcionalidades pendientes en el backlog porque no ha habido tiempo para implementarlas. El agente en la nube de GitHub Copilot puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una solicitud de incorporación de cambios preliminar para que la revise un desarrollador. Así se pueden delegar tareas tediosas, acelerar el desarrollo y dejar que los desarrolladores se centren en tareas de mayor alcance.

Con el agente en la nube de Copilot explorarás:

- cómo personalizar el entorno para generar código.
- cómo garantizar que las operaciones se realicen de forma segura.
- la importancia de delimitar bien las incidencias.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado deficiente: no hay documentación. Como primer paso, el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. En la página de detalles de cada juego se mostrarán otros títulos de la misma categoría para que los visitantes puedan seguir explorando. No hace falta una implementación pulida; solo algo que permita hacer pruebas de aceptación de la experiencia de usuario. Esta tarea bloquea el trabajo, pero hay otras incidencias de mayor prioridad.

Estas tareas pueden quedar relegadas fácilmente y son buenas candidatas para asignarlas al agente en la nube de Copilot. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas y, después, revisar los resultados.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma similar a un desarrollador. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Tras la asignación, Copilot crea una solicitud de incorporación de cambios preliminar para mostrar el progreso, configura el entorno y empieza a trabajar. Puedes consultar la sesión mientras se ejecuta o después de que termine. Cuando la solución esté lista para revisión, Copilot te mencionará en la solicitud de incorporación de cambios.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no es magia: no elimina la necesidad de pensar. Incluso las tareas que parecen sencillas pueden ser complejas cuando se analizan en detalle. Por eso, [delimita cuidadosamente las tareas para el agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al incorporar IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has visto cómo guían a Copilot las instrucciones personalizadas. Si ya has completado el ejercicio práctico sobre instrucciones, has usado `.github/copilot-instructions.md`; si no, este es un buen momento para echarle un vistazo. Antes de asignar tareas al agente en la nube, revisa los archivos de instrucciones existentes en modo de solo lectura para poder observar su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un Codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, sobre todo las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Al asignar la incidencia `Code lacks documentation` al agente en la nube, observa si la solicitud de incorporación de cambios incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas pautas proceden de los archivos de instrucciones. Volveremos a ello al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente en la nube de Copilot

La generación de código suele requerir un entorno específico y la ejecución de scripts de configuración. Esto también se aplica a las tareas que se asignan a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] para configurar el entorno. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow] especial en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo. Así se instalan las herramientas y dependencias necesarias. El flujo ya está configurado para este taller: instala Node.js y las dependencias del proyecto, los navegadores de Playwright para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro.

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Concede solo los permisos mínimos que necesiten *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (ahora toda la aplicación usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos iniciales en la base de datos SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier flujo de trabajo de GitHub, pero hay algunos detalles importantes:

- Contiene un único trabajo, `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El desencadenador `workflow_dispatch` permite ejecutar el flujo manualmente desde la pestaña **Actions** del repositorio. Así se puede comprobar que funciona antes de esperar a que Copilot lo ejecute.

## Añade documentación

Aunque todos saben que la documentación es importante, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica puede ralentizar el trabajo y dificultar el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot puede ayudar a crear documentación, así que esta es una buena incidencia para asignarle. Trabajará en segundo plano y podrás revisar los resultados en un ejercicio posterior.

1. En una pestaña nueva del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de creación de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) que acaba de crear. Su título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la PR. Copilot está configurando el entorno, ejecutando el flujo de trabajo anterior, analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la solicitud de incorporación de cambios y busca una actualización que indique que Copilot ha empezado a trabajar.
15. Selecciona **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot resuelve el problema: explora el código, analiza el estado, se detiene para decidir el enfoque y crea código.

El proceso puede tardar varios minutos. Una ventaja del agente en la nube de Copilot es que trabaja de forma asíncrona y te permite centrarte en otras tareas. Aprovecharemos esta función asignándole otra tarea y, después, trabajaremos en una funcionalidad de la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una ventaja del agente en la nube es que permite dividir el trabajo: puedes centrarte en una tarea mientras Copilot se ocupa de otra. Asignemos al agente la tarea de añadir juegos relacionados para el equipo de diseño.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot**.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, aparecerá el emoji 👀 en el primer comentario de la incidencia para indicar que Copilot está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. Como el agente en la nube trabaja de forma parecida a un ingeniero de software, no hace falta supervisarlo continuamente; revisa el resultado cuando termine. Ahora puedes crear y usar agentes personalizados.

## Resumen y próximos pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona y te ayude a resolver deuda técnica, crear funcionalidades o migrar código entre frameworks.

Has explorado estos conceptos:

- personalizar el entorno para generar código.
- garantizar que las operaciones se realicen de forma segura.
- delimitar bien las incidencias.
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y usar agentes personalizados. El agente en la nube de Copilot también puede [usar servidores MCP][cloud-agent-mcp] y las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
