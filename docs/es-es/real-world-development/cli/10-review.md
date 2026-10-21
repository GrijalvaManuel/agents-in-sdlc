---
title: "Lección 10 - Repaso y próximos pasos"
description: "Repasa el flujo de trabajo con Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo continuo de Tailspin Toys. En concreto:

- has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y localizado la incidencia de filtrado inicial.
- has añadido las valoraciones con estrellas, revisado el resultado en un navegador mediante el puerto reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR).
- has partido de la incidencia de filtrado, definido el enfoque en el modo Plan, implementado la funcionalidad en Autopilot y revisado el resultado en Interactive.
- has guiado al agente con instrucciones personalizadas, personalizado la habilidad `quality-checks` existente y usado la habilidad para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has usado para explorar el filtrado en un navegador real.
- has creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador.
- has revisado el cambio completo del filtrado y autorizado Agent Merge para la PR correspondiente.
- has explorado los comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Qué has entregado

El taller incluye dos hitos de PR, cada uno en su propia rama basada en `main` actualizado:

1. **Valoraciones con estrellas:** muestra `starRating` y un estado explícito para los juegos sin valoración en las tarjetas.
2. **Filtrado y flujo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas relacionadas.

Has usado la misma conversación y rama desde la planificación del filtrado hasta la apertura de su PR. Hemos agrupado el trabajo en una PR para simplificar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, inspeccionando el sitio en el navegador y usando Copilot para explorar el navegador mediante MCP. La habilidad `quality-checks` ha ejecutado pruebas unitarias, lint y comprobaciones de tipos, y ha generado un informe con el nuevo formato. QA ha reunido estos resultados con la revisión de los requisitos y la cobertura de las pruebas antes de abrir la PR.

Las pruebas añadidas deben cubrir carencias reales; si una ejecución de QA concluye que no hacen falta pruebas nuevas, también puede ser un resultado válido. Revisa el código y las pruebas antes de autorizar la combinación, y vuelve a generar las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y adáptalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados reales de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para pensar el enfoque antes de empezar, **Interactive** para participar en los cambios acotados y **Autopilot** para las tareas bien delimitadas. Elige un modelo más rápido para las ediciones habituales y otro más capaz para el trabajo complejo.

El contexto sigue siendo tan importante como la infraestructura. Describir claramente *qué* quieres desarrollar, *por qué* y *cómo* cambia de forma significativa el resultado.

## Más funciones para explorar

Has completado el flujo de trabajo principal. También puedes explorar otras funciones de la CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar subtareas independientes en paralelo.
- `/worktree` para aislar una tarea distinta.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Próximos pasos

La mejor manera de mejorar con cualquier herramienta es seguir usándola. Úsala para código de producción, proyectos personales o esa pequeña aplicación que llevas años queriendo crear. Comparte lo que aprendas con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Backer Concierge con Foundry][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
