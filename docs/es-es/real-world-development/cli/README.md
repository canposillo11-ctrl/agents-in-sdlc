---
slug: es-es/real-world-development/cli
title: "GitHub Copilot CLI"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** incorpora GitHub Copilot a tu terminal como asistente de programación con agentes. Esta interfaz de línea de comandos (CLI) explora bases de código, genera código, ejecuta comandos y se conecta a herramientas externas, para que puedas mantener el flujo sin cambiar a un editor gráfico.

A lo largo de estos ejercicios instalarás Copilot CLI, añadirás una valoración con estrellas y crearás una funcionalidad de filtrado a partir de una incidencia. Usarás los modos Plan y Autopilot, instrucciones personalizadas, una habilidad de comprobaciones de calidad, el servidor Model Context Protocol (MCP) de Playwright y un agente de control de calidad (QA). Después revisarás la solicitud de incorporación de cambios (PR), usarás Agent Merge y explorarás comandos de barra para gestionar el contexto, los modelos y el uso compartido. También puedes seguir una [serie opcional de tres lecciones con GitHub Copilot CLI y Foundry][foundry] para preparar un modelo, crear y publicar un agente hospedado e integrarlo en el sitio.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea tu repositorio y tu codespace |
| [1. Instalación de Copilot CLI][ex1] | Instalación | Instala y autentica Copilot CLI |
| [2. Añadir valoraciones con estrellas][ex2] | Primera modificación | Muestra las valoraciones existentes en las tarjetas de juegos |
| [3. Modos de agente: Plan y Autopilot][ex3] | Generación de código | Planifica e implementa el filtrado a partir de una incidencia |
| [4. Guiar a Copilot con instrucciones personalizadas][ex4] | Contexto | Actualiza las instrucciones y aplícalas al código de filtrado |
| [5. Habilidades de agente][ex5] | Habilidades | Mejora Copilot con habilidades especializadas |
| [6. Validar la funcionalidad con Playwright MCP][ex6] | Herramientas externas | Explora el filtrado en un navegador |
| [7. Crear y usar un agente de QA][ex7] | Agentes | Comprueba los requisitos, las pruebas y el comportamiento en el navegador |
| [8. Crear y combinar la PR de la funcionalidad][ex8] | Entrega | Revisa el cambio y deja que Agent Merge gestione la combinación |
| [9. Comandos de barra en GitHub Copilot CLI][ex9] | Funciones de CLI | Explora el contexto, los modelos, el uso compartido y la delegación opcional |
| [10. Repaso y siguientes pasos][ex10] | Resumen | Repasa el flujo de trabajo y los próximos pasos |
| [Opcional: incorpora Foundry][foundry] | Agentes hospedados | Prepara un modelo, crea y despliega el concierge y conéctalo al sitio web en tres módulos |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo de **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Conocimientos básicos de operaciones de terminal/línea de comandos
- [ ] Git instalado y configurado

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que utiliza este taller, así que puedes completar con él todos los recorridos.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Si usas Copilot Business o Copilot Enterprise, asegúrate de que tu administrador haya habilitado Copilot CLI.

## Primeros pasos

**[Empieza con el ejercicio 0: Requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[foundry]: 8-foundry-agent/
[ex10]: 10-review/
