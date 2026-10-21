---
title: "Lección 5 - Personalizar y usar una habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste solo en escribir código. Hemos comprobado manualmente que funciona y hemos usado archivos de instrucciones para que siga los estándares. Pero ¿qué pasa con las pruebas, lint y las demás partes de la integración continua (CI)?

Para este tipo de tareas, las **habilidades de agente** son la opción adecuada. Las habilidades ayudan a Copilot a entender cómo ejecutar correctamente estas operaciones.

En esta lección:

- explorarás la habilidad `quality-checks` existente.
- personalizarás el formato de los resultados.
- volverás a cargar y ejecutar la habilidad.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables, scripts ejecutables y recursos de apoyo que el agente carga cuando los necesita. En esencia, son una carpeta con el nombre de la habilidad y un archivo Markdown `SKILL.md`. El archivo incluye frontmatter con el nombre y la descripción de la habilidad, una descripción general y pautas sobre cuándo debe usarse. La carpeta también puede incluir scripts y otros recursos.

> [!NOTE]
> No hacen falta archivos ni carpetas adicionales. La habilidad `quality-checks` de Tailspin Toys solo incluye `SKILL.md`, porque usa los comandos existentes del proyecto.

Las habilidades pueden estar en `.github/skills` para compartirse con el equipo o en la carpeta de habilidades del usuario, `~/.copilot/skills`.

## Explora la habilidad

Explora la habilidad `quality-checks` que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo usar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot para ejecutar las pruebas y lint.
4. Fíjate en que la habilidad aún no tiene una sección **Results output formatting**.

## Ejecuta la habilidad antes de hacer cambios

Las habilidades se pueden invocar directamente desde Copilot CLI o mediante lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía esta instrucción:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe que aparece al final.

## Personaliza el informe

Mejora el informe para que indique qué se ejecutó, si terminó correctamente y qué resultados proporcionaron las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final del archivo esta sección:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Copilot CLI puede volver a cargar las habilidades modificadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente la misma instrucción que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Lee el informe y compáralo con el primero.

## Resumen y próximos pasos

Has personalizado y usado una habilidad de agente existente. En esta lección:

- has explorado la habilidad `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos.
- has personalizado el formato de los resultados.
- has vuelto a cargar y ejecutar la habilidad.

La actualización acompañará al filtrado en la PR de la funcionalidad. A continuación, pedirás a Copilot que interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
