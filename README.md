# .github

Repo especial de la organización — GitHub lo trata distinto por su nombre exacto (`.github`), no por convención nuestra: de aquí heredan automáticamente todos los demás repos de `Vidra-ai` sus plantillas de Issues/PR y las plantillas de workflow que aparecen al crear uno nuevo (pestaña "Actions" → "New workflow"). Solo puede existir uno por organización.

Este repo tiene la propiedad `metodologia = viejo` (decisión de sesión, 2026-09-23) — no lleva el PR obligatorio del `ADR-0004` de `vidra-migracion`. No es una elección sobre qué metodología describe su contenido (de hecho aquí conviven las dos, ver abajo), es solo que, al no poder haber un segundo repo `.github` para la metodología nueva, no tiene sentido bloquearlo igual que a un repo de código con mucho más movimiento.

## Qué hay aquí

- **`CONTRIBUTING.md`, `.github/ISSUE_TEMPLATE/`, `.github/issue-branch.yml`** — la metodología de gestión de tareas (Issues, ramas automáticas, el tablero "Vidra — Tareas"). Ver `CONTRIBUTING.md`.
- **`workflow-templates/`** — plantillas del pipeline estándar de Vidra (ADR-0010 de `vidra-migracion`: Preview/Staging/Production, 3 momentos). Aparecen como tarjetas seleccionables en la pestaña "Actions" de cualquier repo nuevo de la organización:
  - **Vidra — Checks en rama** (momento 1)
  - **Vidra — Checks en main** (momento 2)
  - **Vidra — Suite de tests (esqueleto)** — el TODO a rellenar con los comandos reales de cada proyecto
  - **Vidra — Desplegar a producción** (momento 3) — un archivo casi vacío que solo llama al workflow reutilizable de abajo
- **`.github/workflows/deploy-production.yml`** — el workflow reutilizable de verdad (`workflow_call`) para el momento 3: verificar el tag, comprobar que Main Pipeline pasó, disparar los Deploy Hooks de Render. Centralizado aquí porque no cambia nada de un proyecto a otro (a diferencia de los momentos 1 y 2, que sí necesitan referenciar el `test-suite.yml` propio de cada repo — GitHub no permite que un workflow reutilizable alojado aquí referencie un archivo del repo que lo llama, por eso esos dos siguen siendo plantilla-para-copiar, no una referencia en vivo).
- **`.github/workflows/cleanup-ghcr-untagged.yml`** — limpieza semanal de versiones de paquete sin tag en GHCR (incidente del 20/08/26, ver el propio archivo).

## Usar el pipeline estándar en un repo nuevo

1. Actions → New workflow → elegir las 4 tarjetas "Vidra — ...".
2. Rellenar "Vidra — Suite de tests" con los comandos reales (lint/test/build de ese proyecto).
3. Crear el secreto `DEPLOY_HOOK_URLS` (Settings → Secrets and variables → Actions) con las URLs de los Deploy Hooks de Render a disparar, una por línea.
