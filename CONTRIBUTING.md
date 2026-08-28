# Cómo trabajamos en Vidra

Metodología ágil ligera para todos los proyectos de la organización. Esto aplica por igual a cualquier repo nuevo, no solo al que lo motivó (el CRM interno, `V-030-26`).

## Tareas = Issues

Cada tarea es un Issue, nunca una nota suelta o un mensaje de chat. Al crear uno, GitHub te ofrece 4 plantillas (vienen heredadas de este mismo repo `.github`, no hay que copiarlas a cada proyecto):

- **Feature**: nueva funcionalidad. Estructura: Qué incluye / No incluye / Hecho cuando.
- **Bug**: reportar un fallo observado, antes de saber la causa (lo rellena quien se lo encuentra). Estructura: Qué falla / Cómo reproducirlo / Hecho cuando.
- **Fix**: corrección de un bug ya diagnosticado (lo abre quien va a hacer el trabajo — a menudo tras triar un Bug). Estructura: Qué falla / Causa / Hecho cuando.
- **Chore**: mantenimiento o infraestructura sin cambio de negocio.

Cada plantilla aplica su label correspondiente (`feature`/`bug`/`fix`/`chore`) — si un repo nuevo no las tiene creadas, hay que darlas de alta a mano una vez (Issues → Labels → New label), no se heredan de las plantillas de repo (`_plantilla-*`).

## Ramas

Prefijo según el tipo de la tarea: `feat/`, `fix/`, `chore/`, seguido del número de issue y un resumen corto en minúsculas con guiones. Un Bug también genera rama con prefijo `fix/` (arreglarlo es un fix, aunque se haya reportado con la plantilla de Bug).

**Se crean solas**: al asignarte un issue (no al crearlo, a propósito — evita ramas huérfanas de issues que se cierran sin trabajar), un workflow (`create-issue-branch`, configurado en `.github/issue-branch.yml` de este mismo repo `.github`) genera la rama automáticamente con el prefijo correcto según la label.

**Las ramas nunca se borran**, ni a mano ni por automatismo — norma permanente de la empresa.

## Pull Requests

Al abrir el PR, usa la plantilla que aparece sola y escribe en la descripción:

```
Closes #<número del issue>
```

(también valen `Fixes`/`Resolves`). Al mergear a la rama por defecto (`develop`), el issue se cierra solo y su tarjeta se mueve a "Done" en el tablero.

## El tablero: "Vidra — Tareas"

Un único Project de organización para todos los proyectos — **no crear un tablero por proyecto**. Se distingue con dos campos:

- **Proyecto**: código `V-XXX-YY` + nombre. Se añade una opción nueva cada vez que arranca un proyecto.
- **Empresa**: cliente al que sirve el proyecto (`Area`, `Argenia`, `Solinat`, `Vidra (interno)`...). Es un dato del proyecto, no de la tarea — se pone una vez por proyecto (selección múltiple + aplicar en bloque en la vista Table), no issue por issue.

Vistas útiles ya montadas o recomendadas: una por proyecto activo (filtrada por `Proyecto`), **"Sin proyecto asignado"** (filtro `Proyecto is empty` — red de seguridad, revisar cada semana) y **"Mío"** (filtro por Assignee).

### Checklist para dar de alta un proyecto nuevo

1. Crear el repo desde el Panel de Despliegue (hereda solo workflows/labels si están en las 3 plantillas — las labels hay que crearlas a mano, ver arriba).
2. Añadir la opción del proyecto al campo **Proyecto** del tablero.
3. Añadir la opción de la empresa al campo **Empresa** si es un cliente nuevo.
4. Crear los issues de las tareas iniciales, añadirlos al tablero, rellenar ambos campos.
5. Opcional: crear una vista guardada filtrada a ese proyecto.

## Frontera con NocoDB

NocoDB (base "Proyectos y Tareas") es la fuente de verdad del **portafolio**: qué proyectos existen y en qué fase están (`.0` Estudio → `.3` Mantenimiento). GitHub Issues/Projects es la capa de **ejecución**: las tareas concretas dentro de un proyecto que NocoDB ya dice que existe.

No dupliques tareas en NocoDB. Si en algún momento alguien sin cuenta de GitHub necesita ver el estado de las tareas, se resuelve con una vista de solo lectura, no con un segundo registro a mantener sincronizado a mano.
