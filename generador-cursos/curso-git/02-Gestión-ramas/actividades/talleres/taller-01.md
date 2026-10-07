# Taller 01 — Ramas y primer remoto

## Objetivo

Gestionar ramas de principio a fin (RA-1, RA-3) y dar el primer paso hacia la
colaboración remota (RA-2, RA-4), continuando el proyecto del Taller 01 de la Clase
01 — Fundamentos Git.

## Contexto

Vas a seguir desarrollando el repositorio que armaste en la Clase 01, ahora
organizando los cambios en ramas y publicándolo en un repositorio remoto en GitHub.

## Pasos

1. Crear una rama nueva para una mejora o cambio puntual de tu proyecto.
2. Trabajar en esa rama y confirmar al menos un commit.
3. Volver a la rama principal, fusionar la rama y eliminarla.
4. Crear un repositorio remoto nuevo en GitHub (sin inicializarlo con README).
5. Conectar tu repositorio local con el remoto (`git remote add origin ...`) y
   verificar la conexión (`git remote -v`).
6. Publicar tu historial completo en el remoto (`git push -u origin master`).
7. Hacer un cambio directamente desde la interfaz web de GitHub (por ejemplo, editar
   tu `README.md`) y confirmarlo ahí.
8. Traer ese cambio a tu repositorio local (`git pull origin master`).

## Entregable

Un repositorio con al menos una rama creada, trabajada y fusionada, conectado a un
repositorio remoto en GitHub, con el historial local y remoto sincronizados.

## Criterios de evaluación

- La rama se creó, se usó y se fusionó correctamente, sin perder commits.
- El repositorio remoto existe en GitHub y está conectado al local.
- El cambio hecho desde la interfaz web de GitHub llegó correctamente al
  repositorio local tras el `pull`.
