# Básico 01 — Clonar un repositorio

## Estado inicial

La URL del repositorio de ejemplo público que te dio el docente (o un repositorio
`--bare` local si practicás sin conexión). No tenés ninguna copia en tu máquina.
`pull.rebase` ya está configurado en `false`.

## Proceso esperado

1. Clonar el repositorio de ejemplo usando el nombre de carpeta por defecto.
2. Clonar el mismo repositorio por segunda vez, ahora en una carpeta con un nombre que
   vos elijas.
3. Dentro de uno de los clones, comprobar a qué URL apunta `origin`, cuántos commits
   tiene el historial y qué ramas remotas existen.
4. Intentar clonar de nuevo en la carpeta que ya existe y anotar qué dice Git.

## Resultado esperado

- Dos carpetas con el mismo proyecto, una con el nombre del repositorio y otra con el
  nombre que elegiste.
- Podés indicar la URL de `origin`, el número de commits y los nombres de las ramas
  remotas, sin haber ejecutado ningún comando de conexión manual.
- Has visto el mensaje de Git al clonar sobre una carpeta existente y sabés cómo
  evitarlo.

## Restricciones

- No usar `git init` ni `git remote add`: el objetivo es que el clon ya venga conectado.
- No copiar carpetas a mano.

## Resultados de aprendizaje

- RA-1: Clonar un proyecto desde un repositorio remoto en la máquina local.
