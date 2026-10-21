---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente en la nube de Copilot →][next-lesson]

El contexto es fundamental cuando se trabaja con IA generativa. Si una tarea debe hacerse de una manera concreta, o Copilot necesita información de contexto, asegúrate de que pueda acceder a ella. Los [archivos de instrucciones][instruction-files] proporcionan estas pautas para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo*.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las pautas de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones asociados a rutas.
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás las instrucciones antes y después de modificarlas. El agente en la nube de Copilot trabaja de forma asíncrona en incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás la influencia del archivo de instrucciones más adelante, al revisar las solicitudes de incorporación de cambios que genere el agente.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene pautas y requisitos para las prácticas de desarrollo:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe usar un tema oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe comenzar con un bloque de comentarios que describa su función.

Con los archivos de instrucciones proporcionarás a Copilot la información necesaria para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Te permiten orientar las sugerencias y los fragmentos de código. Puedes especificar convenciones de programación, bibliotecas y los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot con **todas** las solicitudes del repositorio. Debe incluir información del proyecto relevante para la mayoría de las interacciones, como la tecnología utilizada, una descripción del proyecto, las prácticas recomendadas y otras pautas generales.
- `.github/instructions/*.instructions.md`: archivos que contienen pautas para tareas o tipos de archivo concretos, como TypeScript, Astro, componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni sugerir ediciones posteriores.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que incluyen frontmatter `applyTo`.
>
> Copilot también [admite otros formatos de archivos de instrucciones][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en pautas generales del proyecto, como la descripción y estructura del proyecto y los estándares de programación.
- Usa archivos `*.instructions.md` para pautas específicas de tipos de archivo, como pruebas unitarias, componentes de Astro o la capa de datos, o para tareas concretas.
- Usa lenguaje natural y pautas claras. Incluye ejemplos de cómo debe ser el código y de cómo no debe ser.

No hay una única manera de crear archivos de instrucciones ni de usar la IA. Experimenta para descubrir qué funciona mejor en tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. En este proyecto encontrarás archivos para varios tipos de tareas, como [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada interfaz ofrece esta función de manera distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El taller de cada interfaz indica cuándo usarla.
>
> Para buscar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones que incluye este repositorio: un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para tareas distintas. Ábrelos en tu editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y observa la descripción breve del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las pautas anidadas de **GitHub Actions Workflows**. Se aplican a todas las interacciones con Copilot en este repositorio.
3. Abre la carpeta `.github/instructions` y explora su contenido. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, las pruebas y otros elementos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: define un patrón glob, relativo a la raíz del repositorio, que determina los archivos a los que se aplican las instrucciones. Por ejemplo, coincide con cualquier archivo de pruebas TypeScript como `**/*.test.ts`.
5. Fíjate en las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y proporcionar ejemplos a Copilot. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero aún no exige documentar el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de archivo.

## Añade un estándar para todo el repositorio

Ahora añadirás una regla general para que la documentación esté en el código, en comentarios TSDoc y en un encabezado de archivo. El agente en la nube usará estas instrucciones al trabajar en las incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente en la nube lee las instrucciones de la rama de destino de la incidencia, pero confirmar los cambios en una rama te permitirá mantener el historial limpio.

1. En el terminal de tu Codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Añade los estándares de programación para mejorar sus sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de programación, pero todavía no incluye una regla para documentar el código.
3. Añade estas líneas Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones pueden estar en `.github/copilot-instructions.md` para pautas generales o en archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares generales como la regla de comentarios que acabas de añadir.

## Confirma, publica y combina la actualización de instrucciones

El agente en la nube lee las instrucciones de la rama de destino de la incidencia. En el siguiente ejercicio, la incidencia se asignará a Copilot y el agente creará una rama a partir de `main`. Por tanto, los cambios deben llegar a `main` para que el agente los use.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y combínala. El agente en la nube leerá estas instrucciones al trabajar en las incidencias del siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar los cambios en `main`. La rama permite practicar un flujo de trabajo habitual en proyectos reales.

## Resumen y próximos pasos

Has explorado cómo obtiene Copilot el contexto de los archivos de instrucciones del proyecto y has añadido un estándar para todo el repositorio a `.github/copilot-instructions.md`. Verás cómo se aplica en las solicitudes de incorporación de cambios que genere el agente en la nube durante el resto del recorrido.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
