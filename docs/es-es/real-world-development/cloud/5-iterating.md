---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

A lo largo de este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que añadiera documentación al código, creara una función de juegos relacionados para que el equipo de diseño pudiera iterar y añadiera funciones de accesibilidad, incluidos controles para los modos de alto contraste y claro. El objetivo es revisar los cambios de código y, si hace falta, pedir a Copilot que los mejore antes de habilitar la revisión por parte del resto del equipo.

### Escenario

Como ya hemos señalado, los fundamentos del diseño de software, desarrollo y operaciones (DevOps) no cambian con la inteligencia artificial (IA) generativa. Siempre debes revisar el código generado y seguir el proceso habitual de DevOps. Por eso, las propuestas de GitHub Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad deben revisarse antes de habilitar la revisión por parte del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de GitHub Copilot en la nube trabaja de forma asíncrona y sin supervisión, hay restricciones de seguridad para proteger el entorno:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- El agente trabaja en GitHub Actions dentro de un entorno independiente y efímero.
- Un usuario debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que se ejecute.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores Model Context Protocol (MCP).

## Revisar la documentación generada

La primera solicitud de incorporación de cambios (PR) generada por el agente de GitHub Copilot en la nube añade documentación al código. Revísala desde la interfaz habitual de PR de GitHub.com para comprobar el resultado.

> [!NOTE]
> Al revisar la PR, quizá veas una advertencia que indica que el cortafuegos ha bloqueado a GitHub Copilot. Es un comportamiento esperado, ya que el acceso a recursos externos —incluidas las llamadas a servidores MCP externos— está limitado de forma predeterminada. Si quieres, puedes [personalizar o deshabilitar el cortafuegos del agente de GitHub Copilot en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la PR titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la marca **[WIP]**. Espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo aprendido.

4. Cuando la PR esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

   ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios de documentación de TypeScript (TSDoc) y otros cambios. El contenido exacto puede variar.

   Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay un indicador que señale que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo aparecen en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se completarán correctamente. El proceso puede tardar varios minutos.

## Pedir cambios a GitHub Copilot

La colaboración con Copilot en una PR no es un proceso unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios insertados en el código, igual que harías con otros miembros del equipo. Copilot verá el comentario e iniciará otra sesión para resolverlo. Como los resultados no son deterministas, no hay una única petición que debas usar. Por ejemplo, puedes pedir a Copilot que:

- añada a cada archivo de código un encabezado con una breve descripción de su función;
- añada comentarios de documentación TSDoc a los archivos TypeScript y Astro;
- cree un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario para pedir un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otro usuario. Usa una de las ideas anteriores u otra petición relacionada con la documentación del código.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para realizar los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

   ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, verás una confirmación nueva en la PR.
5. Selecciona la pestaña **Files changed** y revisa los cambios.

6. Si hace falta, sigue iterando hasta que estés conforme.
7. Cuando estés listo para finalizar la revisión:

   1. Marca la PR como lista para revisión.
   2. Combínala en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

La PR sobre los juegos relacionados permite comprobar la funcionalidad que Copilot generó para la página de detalles.

1. Ve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Comprueba si hay un indicador que señale que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Comprueba que los flujos aparecen en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se completarán correctamente. El proceso puede tardar varios minutos.
9. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente la función de juegos relacionados. Ve al Codespace, abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes iterar con el agente en la nube para pedir cambios. Por ejemplo, puedes pedir que ajuste cuántos juegos relacionados se muestran o que añada encabezados de comentario y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Para pedir cambios, añade otro comentario en la pestaña **Conversation**; Copilot lo verá e iniciará una sesión nueva.

## Revisar las funciones de accesibilidad

La última revisión corresponde a las funciones de accesibilidad implementadas por el agente personalizado. Esta PR debe incluir tanto el modo de alto contraste que asignaste en el ejercicio 3 como el modo claro que pediste en el panel de control del ejercicio 4.

1. Ve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la marca **[WIP]**. Espera a que termine; el proceso puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** y revisa el código generado.
5. Revisa la implementación y presta atención a:
   - los controles de interfaz para alternar entre modos;
   - el uso del almacenamiento local para conservar las preferencias del usuario;
   - los cambios de hojas de estilo en cascada (CSS) para los modos de alto contraste y claro;
   - los atributos de accesibilidad, como Accessible Rich Internet Applications (ARIA) y la navegación con teclado;
   - el código JavaScript o TypeScript que gestiona el cambio de modo.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay un indicador que señale que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos aparecen en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se completarán correctamente. El proceso puede tardar varios minutos.
10. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Ve al Codespace, abre el terminal y ejecuta estos comandos. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Confirma que funcionan y que las preferencias se conservan al volver a cargar la página.

10. Observa cómo el agente personalizado de accesibilidad ayudó a Copilot a implementar estas funciones siguiendo buenas prácticas de accesibilidad. Si detectas problemas o mejoras posibles, menciónalos en un comentario con **@copilot**, como hiciste con las PR anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube resulta útil para delegar tareas reales del backlog y ocuparte de otras cosas mientras trabaja. Algunas ideas para practicar:

1. Crea un formulario de interés para patrocinadores en la página de detalles del juego.
2. Implementa la paginación en la página de la lista de juegos.
3. Añade validación de entradas y gestión de errores a las funciones de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Revisaste los archivos de instrucciones personalizadas del repositorio** para poder observar después su efecto en el trabajo del agente.
- **Asignaste incidencias al agente de GitHub Copilot en la nube** y observaste cómo configuraba el entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y usaste un agente personalizado** para mejorar la accesibilidad con controles para los modos de alto contraste y claro.
- **Usaste la página de agentes de Copilot como panel de control** para supervisar y dirigir la sesión de accesibilidad mientras estaba en curso.
- **Revisaste e iteraste sobre las PR del agente en la nube**, mencionaste a `@copilot` para pedir cambios y aprobaste los flujos de trabajo.

## Revisión y siguientes pasos

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y el puente opcional `/delegate` hacia el agente en la nube.

Estas ideas también sirven para practicar en tu propio repositorio:

1. Crea más incidencias en el repositorio y asígnaselas a Copilot. Por ejemplo:
   - una incidencia para crear un formulario de interés para patrocinadores en la página de detalles del juego;
   - una incidencia para implementar la paginación en la página de lista de juegos;
   - una incidencia para añadir validación de entradas y gestión de errores a las funciones de acceso a datos.
2. Crea un agente personalizado para otro ámbito.
3. Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente de GitHub Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
