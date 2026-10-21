---
title: "Ejercicio 3 - Prueba la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear el filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En lugar de probar la aplicación manualmente, conectarás el **servidor MCP de Playwright** para que Copilot controle un navegador real y pruebe la funcionalidad. Después publicarás la rama y abrirás la PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo amplían Copilot los servidores MCP.
- añadirás el servidor MCP de Playwright al espacio de trabajo.
- pedirás a Copilot que lo use para probar manualmente la funcionalidad en un navegador.
- publicarás la rama y abrirás una solicitud de incorporación de cambios.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente tiene más capacidades cuando puede acceder a recursos externos al editor. Model Context Protocol (MCP) permite a Copilot comunicarse con herramientas y servicios externos.

![Diagrama que muestra el funcionamiento interno del modo agente y cómo interactúa con el contexto, el LLM y las herramientas, incluidas las herramientas de servidores MCP y extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite a los agentes de IA comunicarse con herramientas y servicios externos. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los agentes acceden a estas herramientas y recursos mediante un servidor MCP, que actúa como puente con servicios externos, como API existentes o herramientas locales, por ejemplo, paquetes de NPM. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona funciones de automatización del navegador, como navegar por páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y considera las implicaciones de seguridad. Usa solo servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones confidenciales.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Deberías ver un servidor `github` ya configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto incluye la entrada `github`. Por eso Copilot pudo consultar el backlog de incidencias en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hace falta instalar nada localmente: VS Code se conecta por HTTP y te autenticas con GitHub la primera vez que Copilot usa una de sus herramientas.

## Añade el servidor MCP de Playwright

Ahora añadirás otro servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona un navegador que Copilot puede controlar para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**: VS Code lo inicia ejecutando `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin una ventana visible, algo necesario en Codespaces, donde no hay un escritorio que lo muestre.

> [!NOTE]
> Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador ya está instalado. Si Copilot indica que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia el servidor y confía en él

VS Code inicia los servidores MCP **cuando se necesitan**. La primera vez que Copilot solicita las herramientas de un servidor, VS Code lo inicia y te pregunta si confías en él.

1. En `.vscode/mcp.json`, aparecerá la acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pide confirmar que confías en el servidor, revisa la configuración y elige confiar para que pueda iniciarse.

Iniciarlo manualmente es opcional. Si no lo haces, VS Code lo iniciará y te pedirá que confirmes que confías en él la primera vez que Copilot necesite sus herramientas.

## Prueba la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona un navegador real que Copilot puede controlar. En lugar de probar tú la aplicación, el agente puede abrir páginas, navegar, aplicar filtros y leer los resultados; después, resumirá lo que haya observado. Así puedes confirmar rápidamente el comportamiento sin salir de la conversación.

El servidor MCP de Playwright usa el [árbol de accesibilidad][playwright-mcp-server] de la página, no capturas de pantalla. El agente razona sobre elementos estructurados y etiquetados, como botones, enlaces y elementos de lista, de forma parecida a la tecnología de asistencia. Así, una comprobación funcional también permite revisar algunos aspectos básicos de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe el filtrado:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot iniciará un navegador mediante el servidor MCP de Playwright, seguirá los pasos y comunicará los resultados. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona, pide más detalles o solicita que corrija el código antes de abrir la PR.

> [!NOTE]
> Para esta prueba, la aplicación debe estar disponible en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar la instrucción. La primera vez que Copilot use Playwright MCP, puede que tenga que descargar un navegador. Si indica que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Una vez comprobado el funcionamiento, abre una solicitud de incorporación de cambios (PR) para que el equipo pueda revisar la funcionalidad. Primero, publica la rama `filtering-vscode`.

1. Abre el panel **Source Control** del Codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para publicar la rama en el repositorio.

## Crea la solicitud de incorporación de cambios

Puedes crear una PR desde github.com o desde la interfaz de línea de comandos de GitHub. Como ya estás trabajando con Copilot, puedes pedirle que encuentre la incidencia correspondiente y cree una PR vinculada.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión.
2. Confirma que **Agent** está seleccionado para permitir que Copilot use las herramientas de GitHub.
3. Pide a Copilot que cree la PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile información y realice acciones. La primera vez que use una herramienta de GitHub, puede que tengas que autenticarte. Sigue las instrucciones.
5. Observa cómo Copilot busca las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace de Copilot para revisar la solicitud de incorporación de cambios, pero **no la combines todavía**.

## Resumen y próximos pasos

En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo amplían Copilot los servidores MCP.
- has añadido Playwright MCP al espacio de trabajo.
- lo has usado para probar manualmente el filtrado en un navegador antes de publicarlo.
- has publicado la rama y abierto una solicitud de incorporación de cambios.

Has usado un servidor MCP para probar una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [¿Qué es MCP y por qué habla todo el mundo de ello?][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro de MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
