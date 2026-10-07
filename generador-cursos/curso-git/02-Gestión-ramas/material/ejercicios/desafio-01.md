# Desafío 01 — Ciclo completo: rama, merge y remoto

## Estado inicial

Un repositorio local propio con al menos un commit. Un repositorio remoto (en
GitHub, o un repositorio `--bare` local si estás practicando sin conexión a
internet).

## Proceso esperado

1. Crear una rama nueva, trabajar en ella (al menos un commit), volver a la rama
   principal y fusionarla; eliminar la rama ya fusionada.
2. Crear un repositorio remoto nuevo y conectarlo con tu repositorio local.
3. Publicar tu historial completo en el remoto.
4. Simular un cambio hecho "desde otro lugar": si usás GitHub, editá un archivo
   directamente desde su interfaz web y confirmá el cambio ahí; si estás practicando
   con un remoto `--bare` local, cloná ese remoto en **otra carpeta**, hacé un commit
   ahí, y publicalo.
5. Volver a tu repositorio local original y traer ese cambio.
6. Usar `git log --oneline --all --graph` para mostrar el historial final completo.

## Resultado esperado

- El historial local tiene el commit de la rama fusionada.
- El repositorio remoto existe y está conectado (`git remote -v` lo confirma).
- El cambio hecho "desde otro lugar" aparece en tu repositorio local después del
  `pull`.
- `git log --oneline --all --graph` muestra un historial lineal o con una fusión,
  según cómo se haya dado la sincronización, pero sin ningún commit perdido.

## Restricciones

- Ningún commit puede tener un mensaje genérico ("cambios", "update").
- El cambio "desde otro lugar" tiene que confirmarse en una copia **distinta** del
  repositorio (otra carpeta, otro clon), no en la misma carpeta de tu repositorio
  original.

## Resultados de aprendizaje

- RA-1: Entender el funcionamiento de las ramas en Git.
- RA-2: Generar un repositorio remoto.
- RA-3: Hacer la debida gestión con la creación, eliminación y merge de ramas.
- RA-4: Trabajar de forma conjunta entre un repositorio local y uno remoto.

## Casos de prueba

| Acción | Resultado esperado |
|---|---|
| `git branch` después del paso 1 | Solo queda la rama principal; la rama de trabajo ya no existe (fue fusionada y eliminada) |
| `git remote -v` después del paso 2 | Muestra `origin` con la URL o ruta correcta |
| `git log --oneline` en el remoto, tras el paso 3 | Incluye el commit de la rama fusionada |
| `cat <archivo>` en el local, tras el paso 5 | Incluye el contenido agregado "desde otro lugar" |
