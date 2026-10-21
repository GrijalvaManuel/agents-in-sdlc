---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o ámbitos concretos del flujo de desarrollo. Al definir agentes en archivos Markdown de `.github/agents`, puedes proporcionar instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos del dominio. Los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo incluye frontmatter YAML con, al menos, `name` y `description`, seguido de una instrucción Markdown que define el comportamiento, los conocimientos y las pautas del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] tienen algunas similitudes: ambos se definen principalmente en Markdown y explican a la IA cómo realizar tareas. La forma más sencilla de distinguirlos es que el **agente personalizado** realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades e incluso otros agentes. En este taller, el agente personalizado de accesibilidad revisa y actualiza el sitio según las pautas de accesibilidad; puede usar habilidades como un flujo de trabajo de solicitudes de incorporación de cambios o una que ejecute y gestione pruebas.

> [!NOTE]
> No hay una única manera correcta de crear un agente personalizado. Como con cualquier función de IA, prueba e itera para descubrir qué funciona en tu entorno y tus escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio aprenderás:

- cómo se definen los agentes personalizados.
- cómo asignarles una tarea.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todos, independientemente de sus capacidades visuales o preferencias. Algunos usuarios han comentado que el tema oscuro actual es difícil de leer porque hay poco contraste entre el texto y el fondo. Para resolverlo, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

La accesibilidad es prioritaria, así que usarás un agente personalizado para implementar esta función cuanto antes.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

Vuelve a tu Codespace y abre un terminal. Crea una rama nueva a partir de `main` para mantener separado el trabajo de accesibilidad de la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Lee las secciones siguientes, que incluyen:
   - Responsabilidades principales al generar código para un sitio web accesible.
   - Prácticas recomendadas de accesibilidad.
   - Ejemplos de código HTML, CSS y JavaScript.
   - Errores y problemas habituales.

## Usa el agente personalizado en Copilot Chat

VS Code muestra los agentes personalizados definidos en `.github/agents` en el selector de agentes, en la parte inferior de la vista Copilot Chat. Al seleccionar uno, se limita la sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo pueden variar según el ejercicio; sigue las indicaciones de cada paso.

1. Si todavía no está seleccionado, elige **Agent** en el selector de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Abre el selector de agentes situado en la parte inferior de la vista Chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista.
4. Envía esta instrucción al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y un diff que puedes revisar.
6. El indicador **Files changed** se actualizará a medida que el agente modifique archivos.

> [!NOTE]
> El proceso puede tardar unos minutos. Copilot hará cambios reales en el repositorio: puede editar componentes Astro, CSS y pruebas relacionadas.

En el siguiente ejercicio revisarás y guiarás el trabajo en curso.

## Resumen y próximos pasos

Has explorado los [agentes personalizados][custom-agents], asistentes de IA especializados para tareas y ámbitos concretos. Permiten plasmar los conocimientos y estándares del equipo en agentes reutilizables que orientan a Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo usar un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un control de modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
