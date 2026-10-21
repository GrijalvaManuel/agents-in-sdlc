---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para implementar el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto, pero los cambios más grandes requieren un proceso más sólido. GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de una organización y ayudar a crear lo adecuado de la forma correcta. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es correcto y que la funcionalidad se comporta como se espera, y finalmente la combinarás en el proyecto.

> [!NOTE]
> A medida que avances con esta funcionalidad, usarás la misma conversación y rama. Normalmente utilizarías ramas o PR distintas para los diferentes tipos de archivos, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación nueva de Copilot a partir de una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás manualmente la funcionalidad en un navegador mediante el puerto reenviado.

Mientras continúas con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás la validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar algunos detalles, como la combinación de categorías. Usarás el modo Plan para tomar esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia sus fundamentos; de hecho, los hace aún más importantes. La mayoría de los desarrolladores sigue un proceso parecido a este:

1. Partir de una incidencia registrada que describa el trabajo.
2. Crear un plan con lo que hay que desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Cuando se haya revisado el código y se hayan superado las comprobaciones de integración continua, combinar el código.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos se parecen al descrito.

Seguir este proceso habitual permite comprobar que el código generado por IA cumple los requisitos y pasa por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: tú y el agente trabajáis en equipo. El agente propone cambios y espera tu respuesta antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas y realiza iteraciones hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Parte de la incidencia

Antes de empezar el trabajo de filtrado, vuelve a tu Codespace y comprueba que el repositorio y el terminal estén listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que la PR de las valoraciones con estrellas se ha combinado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa dos veces <kbd>Tab</kbd> para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia a la instrucción y volver a la pestaña **Session**.

Observa que la instrucción empieza ahora por `#7` (o un número parecido). El carácter `#` permite incluir una incidencia o solicitud de incorporación de cambios (PR) de GitHub en el contexto.

## Planifica la funcionalidad de filtrado

La planificación te permite definir el enfoque antes de pedir a Copilot que implemente una funcionalidad o realice una tarea. Siempre es buena idea dedicar algo de tiempo a planificar las tareas complejas. Cambia al modo Plan y pide a Copilot que elabore el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador bajo la instrucción muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, escribe esta instrucción:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empezará a explorar el proyecto y a determinar el mejor enfoque para elaborar el plan.

3. Durante el proceso, Copilot puede hacer preguntas sobre el funcionamiento del filtrado. Respóndelas según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que modifique cualquier parte que no coincida con tus decisiones.

## Da luz verde a Autopilot

Una vez escrito y revisado el plan, toca implementarlo. Deja que Copilot se encargue usando Autopilot.

Autopilot permite a Copilot realizar iteraciones sobre el problema hasta que considere que está resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción con un nombre similar en la versión instalada.
2. Confirma que el indicador de modo bajo la instrucción muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisa y verifica la implementación

Una vez generado el código, hay que revisarlo antes de combinarlo, como cualquier otro código. Revisa el código y ejecuta el sitio para comprobar que todo funciona correctamente.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Interactive. Confirma que el indicador no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y sus pruebas.
3. Compara el resultado con la incidencia y con las decisiones tomadas durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los errores.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, por editorial y las combinaciones acordadas en el plan.
4. Confirma que el reinicio del filtrado y el comportamiento cuando no hay resultados coinciden con la incidencia y con tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y próximos pasos

Has usado distintos modos de conversación para desarrollar y revisar una funcionalidad. En esta lección:

- has iniciado una conversación nueva de Copilot a partir de una incidencia de GitHub.
- has definido los requisitos en el modo Plan.
- has implementado la funcionalidad con el modo Autopilot.
- has revisado el código.
- has validado manualmente la funcionalidad en un navegador mediante el puerto reenviado.

A continuación, profundizarás en la generación de código y en cómo garantizar que siga las prácticas documentadas [mediante instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
