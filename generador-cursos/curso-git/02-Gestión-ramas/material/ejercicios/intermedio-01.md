# Intermedio 01 — Resolver un conflicto de fusión

## Estado inicial

Un repositorio con un commit en la rama principal, con un archivo de texto cualquiera
(por ejemplo `README.md`).

## Proceso esperado

1. Crear una rama nueva y, en ella, modificar una línea específica del archivo;
   confirmar el cambio en un commit.
2. Volver a la rama principal y modificar **esa misma línea** con un contenido
   distinto; confirmar el cambio en otro commit.
3. Intentar fusionar la rama nueva a la rama principal.
4. Identificar los marcadores de conflicto en el archivo, decidir el contenido final,
   eliminar los marcadores, y completar la fusión.

## Resultado esperado

- `git merge` falla y avisa del conflicto.
- `git status` muestra el archivo como "unmerged".
- El archivo, antes de resolver, contiene los marcadores
  `<<<<<<<`/`=======`/`>>>>>>>`.
- Después de resolver y confirmar, `git status` ya no muestra el conflicto, y el
  archivo tiene el contenido final que elegiste, sin marcadores.

## Restricciones

- No uses `git merge --abort` para evitar el conflicto: el objetivo del ejercicio es
  resolverlo.
- Los marcadores de conflicto no deben quedar en el archivo confirmado.

## Resultados de aprendizaje

- RA-3: Hacer la debida gestión con la creación, eliminación y merge de ramas.
