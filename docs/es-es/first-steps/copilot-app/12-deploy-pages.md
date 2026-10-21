---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y verificar el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica Space Quiz en GitHub Pages para que cualquiera pueda probarlo. Copilot puede preparar el flujo de trabajo y guiar la publicación, pero tú debes revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure la publicación en GitHub Pages.
- revisarás el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla.
- seguirás el proceso de publicación y comprobarás el sitio publicado.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero aún hace falta una forma sencilla de probarlo. Publicar el sitio de un único archivo con GitHub Pages permite acceder a él desde una URL pública sin añadir un servidor ni dependencias de ejecución.

## Pide a Copilot que publique el sitio

En la sesión de Copilot app para `space-quiz`, envía esta instrucción:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama en tres etapas: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de GitHub Actions llamada Deploy website, con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Esta instrucción pide al agente que cree y combine una solicitud de incorporación de cambios y cambie la configuración de Pages del repositorio. Revisa el diff, los permisos de publicación y cualquier solicitud de aprobación de GitHub antes de continuar.

## Revisa el flujo de trabajo y verifica la publicación

1. Confirma que el archivo README solo se añade si el repositorio aún no tiene uno.
2. Revisa los cambios de la solicitud de incorporación de cambios, incluidos los pasos de compilación y publicación del flujo de trabajo.
3. Combínala solo después de que se superen las comprobaciones obligatorias.
4. Sigue la ejecución de publicación de GitHub Actions hasta que termine correctamente.
5. Abre la URL indicada y confirma que se carga Space Quiz.

Si el agente no puede habilitar Pages o combinar la solicitud de incorporación de cambios, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que se complete la publicación y se cargue el sitio.

## Resumen y próximos pasos

Space Quiz ya está disponible como sitio publicado y has verificado la implementación en lugar de depender del informe del agente. Continúa con la [lección 13: Revisión y próximos pasos][next-lesson].

[next-lesson]: ../13-review/
