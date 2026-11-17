---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores has pedido al agente de GitHub Copilot en la nube que realice tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, es importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente, como la [página de agentes][agents-page] de GitHub.com. Desde este panel puedes ver todas las tareas de los agentes con solicitudes de incorporación de cambios (PR) abiertas, consultar las operaciones realizadas e incluso dirigir una sesión en curso.

En este ejercicio:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- dirigirás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo decide añadir también un modo claro. Como el agente ya está actualizando los estilos del sitio y añadiendo controles de alternancia, parece lógico incluir esa función. Dirigirás el trabajo para que añada el modo claro además del modo de alto contraste.

## Revisar las tareas del agente de GitHub Copilot en la nube

Consulta el estado de las tareas asignadas al agente de GitHub Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base;
   - crear interfaces de programación de aplicaciones (API) para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - clonar el código del repositorio;
   - crear el entorno de trabajo de Copilot;
   - configurar servidores Model Context Protocol (MCP);
   - realizar los pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos exactos dependen del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de la solicitud de incorporación de cambios (PR), que aparece a la derecha. Desde ahí puedes consultar la PR y los archivos modificados para seguir el trabajo.

## Dirigir el agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el control de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión que corresponda a la implementación del modo de alto contraste. El título exacto dependerá del nombre que le haya dado Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en el panel de control de agentes](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarea del agente en la nube con el cuadro de diálogo Steer active session while Copilot is working resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la nueva solicitud cuando termine el paso en el que estaba trabajando al recibir el mensaje.

Como antes, el proceso tardará varios minutos. Puedes aprovechar la espera para repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y dirigir las tareas del agente de GitHub Copilot en la nube. Permite seguir las tareas activas y completadas, revisar el trabajo y redirigir tareas en curso para ajustar su alcance o dar instrucciones adicionales.

En concreto:

- exploraste el panel de control y la página de agentes para supervisar las tareas del agente en la nube;
- redirigiste una sesión en curso para pedir una funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
