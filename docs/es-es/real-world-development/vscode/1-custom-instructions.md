---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe realizarse de una manera concreta, o Copilot necesita información de fondo, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] son una de las principales maneras de proporcionar pautas para la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las pautas de programación y los estándares de documentación mediante instrucciones personalizadas y archivos de instrucciones asociados a rutas.
- generarás la primera parte de los datos para el filtrado, una función auxiliar de editoriales, con las instrucciones actuales.
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`.
- volverás a enviar la misma instrucción y observarás cómo el código generado sigue el nuevo estándar.
- confirmarás la actualización de instrucciones y la parte de filtrado en `main` para que Copilot pueda usar las nuevas pautas en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede desviarse de algunos estándares: Copilot no es determinista. El objetivo es observar la tendencia después de actualizar las instrucciones, no obtener siempre exactamente el mismo resultado.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene pautas y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe comenzar con un bloque de comentarios que describa su función.

Con los archivos de instrucciones proporcionarás a Copilot la información necesaria para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot entienda mejor tu estilo y tus requisitos. Puedes especificar convenciones de programación, bibliotecas y los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot con **todas** las solicitudes del repositorio. Debe incluir información general pertinente para la mayoría de las interacciones, como la pila tecnológica, una descripción del proyecto, prácticas recomendadas y otras pautas.
- `.github/instructions/*.instructions.md`: archivos con pautas para tareas o tipos de archivo concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni sugerir ediciones posteriores.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo`.
>
> Copilot también [admite otros formatos de archivos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque posible:

- Mantén `copilot-instructions.md` centrado en las pautas generales, como la descripción y estructura del proyecto y los estándares de programación.
- Usa `*.instructions.md` para dar pautas específicas de tipos de archivo, como pruebas unitarias, componentes Astro o la capa de datos, o de tareas concretas.
- Usa lenguaje natural y pautas claras. Incluye ejemplos de cómo debe ser el código y de cómo no debe ser.

No hay una única manera de crear archivos de instrucciones ni de usar la IA. Experimenta para descubrir qué funciona mejor en tu proyecto.

> [!TIP]
> Todos los proyectos con GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Este proyecto incluye archivos para tareas como [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generarlos. Cada interfaz ofrece esta función de forma distinta, como **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El taller de cada interfaz indica cuándo usarla.
>
> Para buscar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], que incluye archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones del proyecto

Lee los archivos de instrucciones que incluye este repositorio: un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para tareas concretas. Ábrelos en tu editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las pautas anidadas de **GitHub Actions Workflows**. Se aplican a todas las interacciones con Copilot en este repositorio.
3. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo`, que define un patrón glob relativo a la raíz del repositorio. En este caso, coincide con los archivos de pruebas TypeScript, como `**/*.test.ts`.
5. Lee las instrucciones específicas para las pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> **Code formatting requirements** en `copilot-instructions.md` describe los estándares del proyecto, pero aún no exige documentar el código. En los siguientes pasos añadirás reglas para comentarios TSDoc y encabezados de archivo.

## Crea una rama

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indica cuándo son importantes.

Para observar el efecto de las instrucciones personalizadas, primero envía una instrucción con el archivo actual. Más adelante lo actualizarás y volverás a enviar la misma instrucción para ver la diferencia.

1. Cierra las pestañas del editor de ejercicios anteriores para que Copilot solo tenga el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse la función auxiliar.
3. En el selector de agentes de la vista Chat, elige **Agent** para que Copilot pueda editar archivos.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía esta instrucción:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios, a menudo en la función auxiliar y sus pruebas.
6. Observa que la función propuesta es tipada, acepta un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto procede de las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Fíjate en que el código propuesto **no incluye** comentarios TSDoc ni un encabezado de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios aunque no se lo pidas. No pasa nada: lo importante es observar que las instrucciones mejoran la coherencia.

## Añade un estándar para todo el repositorio

`.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade los estándares de programación para mejorar sus sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares del proyecto, pero todavía no incluye una regla para documentar el código.
3. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones pueden estar en `.github/copilot-instructions.md` para pautas generales o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares generales como la regla de documentación que acabas de añadir.

## Vuelve a enviar la instrucción y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar el contexto.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía la **misma instrucción**:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que ahora el archivo empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique al código que usarás en el ejercicio siguiente.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones se gestionan con el mismo control de versiones que los demás archivos del repositorio.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que se hayan actualizado las instrucciones y la función auxiliar:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y todos los archivos modificados por Copilot:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra cambios adicionales de archivos auxiliares (por ejemplo, pruebas), añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Con la rama publicada, crea una solicitud de incorporación de cambios y vincúlala a la incidencia del backlog sobre las instrucciones. Copilot puede buscar la incidencia, crear la PR y combinarla mediante las herramientas de GitHub ya conectadas al proyecto.

> [!NOTE]
> En este ejercicio Copilot actuará por primera vez en GitHub. La plantilla del proyecto ya configura las herramientas de GitHub. La primera vez que Copilot use una, VS Code puede pedirte que inicies sesión. Sigue las instrucciones para permitirlo. En otro ejercicio aprenderás cómo funciona esta conexión con Model Context Protocol.

1. Abre Copilot Chat en el Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que busque la incidencia y cree una PR desde la rama actual, indicando tanto los cambios de instrucciones como la base de la función auxiliar de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si procede.
6. Una vez creada la PR, pide a Copilot que la combine con `main` y actualice la rama local:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y comandos de shell, revisa el comando y selecciona **Allow** si procede.

¡Has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y próximos pasos

Has explorado cómo obtiene Copilot el contexto de los archivos de instrucciones y has usado Copilot Chat en VS Code para:

- enviar una instrucción de generación de código y observar el resultado con las instrucciones existentes.
- añadir un estándar para todo el repositorio a `.github/copilot-instructions.md`.
- volver a enviar la misma instrucción y observar cómo el código propuesto sigue el nuevo estándar.
- confirmar la actualización de instrucciones y la base de la función auxiliar en `main` para el siguiente ejercicio.

A continuación, aplicarás esas instrucciones en el [modo agente][next-lesson] mientras Copilot añade una funcionalidad en varias partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
