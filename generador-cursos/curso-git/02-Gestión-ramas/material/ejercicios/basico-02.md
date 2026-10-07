# Básico 02 — Fusión sin conflicto

## Estado inicial

Un repositorio local con un commit en la rama principal.

## Proceso esperado

1. Crear una rama nueva y, en ella, agregar un archivo nuevo (distinto a los que ya
   existen) y confirmarlo en un commit.
2. Volver a la rama principal y, ahí, modificar un archivo ya existente (distinto al
   que creaste en la otra rama) y confirmarlo en otro commit.
3. Fusionar la rama nueva a la rama principal.

## Resultado esperado

- Antes de fusionar, `git log --oneline --all --graph` muestra las dos ramas
  divergiendo desde un commit común.
- La fusión se completa sin ningún conflicto (porque los archivos modificados son
  distintos).
- Después de fusionar, ambos archivos (el nuevo y el modificado) están presentes.

## Restricciones

- Los dos commits que diverjan deben modificar archivos **distintos** (si modifican
  el mismo archivo en la misma línea, vas a provocar un conflicto, que es el tema del
  ejercicio Intermedio 01).

## Resultados de aprendizaje

- RA-3: Hacer la debida gestión con la creación, eliminación y merge de ramas.
