---
name: tasks-to-issues
description: Convierte la LISTA DE TAREAS de una spec ASDD aprobada en GitHub Issues ordenados por dependencia. Úsalo después de la implementación completa o cuando el equipo quiere rastrear el progreso en GitHub. Requiere que el remoto sea GitHub.
argument-hint: "<nombre-feature> (ej: folio-generator)"
---

# Tasks to Issues

Invoca el agente `tasks-to-issues` pasándole el argumento recibido.

## Prerequisitos

- Debe existir `.claude/specs/<feature>.spec.md` con status `APPROVED`, `IN_PROGRESS` o `IMPLEMENTED`
- El remoto Git debe ser una URL de GitHub

## Proceso

Delega al agente `tasks-to-issues` con el feature indicado en `$ARGUMENTS`.

El agente:

1. Lee la spec y verifica su estado
2. Extrae todos los ítems de `## 3. LISTA DE TAREAS`
3. Ordena por dependencias ASDD (Backend impl → Frontend impl → Backend tests → Frontend tests → QA)
4. Crea un GitHub Issue por cada tarea con labels, contexto y criterios de aceptación
5. Reporta el resumen de issues creados
