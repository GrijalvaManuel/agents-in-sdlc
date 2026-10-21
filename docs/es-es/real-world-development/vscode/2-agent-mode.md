---
title: "Ejercicio 2 - Añade funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones sencillas suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El proceso habitual incluye localizar los archivos, hacer cambios, ejecutar pruebas, depurar y volver a actualizar lo que falte. El modo agente de Copilot puede ayudar a coordinar este trabajo.

El modo agente de Copilot funciona de forma más autónoma en el IDE. Explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. En este ejercicio lo usarás para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite ciclos rápidos de interacción. El agente en la nube trabaja de forma asíncrona, como un miembro del equipo, mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás:

- cómo explora el modo agente el proyecto, identifica los archivos pertinentes y coordina cambios.
- cómo implementar funcionalidades en la interfaz y la capa de datos.
- cómo revisar los cambios y las pruebas antes de combinar el código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya añadiste una función auxiliar para obtener editoriales. Ahora completarás la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web Tailspin Toys

Antes de modificar el código, explora las funciones actuales del sitio.

Tailspin Toys es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo. Permite consultar los juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde el frontmatter mediante Drizzle ORM; no hay una API de backend separada ni un framework de interfaz en el cliente. Las funciones de acceso a datos reutilizables están en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro y las API estándar del DOM.

### Inicia el sitio web

El comando `npm run dev` inicia el servidor de desarrollo de Astro.

1. Vuelve a tu Codespace. Al terminar el ejercicio anterior, deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web:

   ```bash
   npm run dev
   ```

   Cuando esté listo el servidor Astro, verás una dirección parecida a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir `http://localhost:4321` en el navegador, ciérralo con **x**.

5. Para abrir el sitio web, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, seleccionar el enlace de localhost desde el terminal redirige a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explora el sitio web

Las funciones principales son:

- **Página principal**: muestra los juegos de mesa, sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de continuar, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso los indica cuando son importantes.

La primera versión del sitio funciona, pero queremos añadir funciones. Revisa el backlog de incidencias que se creó automáticamente al configurar el proyecto con la plantilla.

1. En el selector de agentes de la vista Chat, selecciona **Agent**. Este agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal y usa herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. En la lista de modelos, selecciona **Claude Sonnet 4.5**.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo sobre otro. Usamos Claude Sonnet 4.5 al crear el taller para que las sugerencias fueran relativamente coherentes, pero los modelos de lenguaje no son deterministas y los resultados pueden variar. Es normal.

> [!NOTE]
> Debido al carácter probabilístico de los modelos, Copilot puede usar un comando MCP distinto, pero debería completar la tarea.

3. Envía esta instrucción para consultar el backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado las incidencias según su utilidad para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las pautas generales, entre ellas:
   - Una descripción de la arquitectura.
   - Principios de diseño de componentes, pruebas y accesibilidad.
   - Enlaces a archivos de instrucciones para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así puedes reutilizar contenido, dividir conjuntos complejos en archivos más manejables y ofrecer ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, habrá que:

- añadir o mejorar la lógica restante en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del filtrado.
- actualizar la página de lista de juegos para incorporar la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios. El modo agente de Copilot puede realizar estas tareas.

1. Continúa en la conversación actual de Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura de la selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** está seleccionado.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado.
5. Envía esta instrucción para implementar la funcionalidad a partir de la incidencia relacionada:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos de la funcionalidad, incluidos los ayudantes de acceso a datos, la interfaz y las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explorará el proyecto, modificará archivos y ejecutará pruebas. El proceso puede tardar varios minutos. Mientras trabaja, pueden aparecer los botones **Keep** y **Undo**. No hace falta seleccionarlos hasta que Copilot termine.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura del cuadro de diálogo de Copilot Chat que solicita permiso para ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede haber pausas y algunas pruebas pueden fallar durante el proceso. Copilot alternará entre la generación de código y la ejecución de pruebas hasta completar la tarea y no detectar errores.

   ![Captura de una sesión completa de Copilot Chat en modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Revisa siempre el código que genera Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la funcionalidad nueva.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y próximos pasos

Has usado el modo agente de GitHub Copilot para añadir funciones a Tailspin Toys. En concreto:

- has visto cómo el modo agente implementa funcionalidades en la interfaz y la capa de datos.
- has explorado cómo identifica los archivos pertinentes y coordina los cambios.
- has revisado los cambios y las pruebas antes de combinar el código.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar la paginación

A medida que crezca la lista de juegos, hará falta paginarla. Usa lo aprendido para pedir a Copilot que implemente la paginación. Ten en cuenta estas pautas:

- Sigue las prácticas existentes, incluidos los archivos de instrucciones.
- Decide cómo quieres implementar la paginación: por ejemplo, si el usuario puede elegir el tamaño de página o si será fijo.
- Proporciona a Copilot las indicaciones necesarias para implementar lo que necesitas.
- Puede que tengas que iterar y proporcionar más contexto. Es normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
