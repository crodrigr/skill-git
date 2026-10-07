# Ejemplo 02 — Repositorio, staging y commit

## Estado inicial

Una carpeta vacía `mi-proyecto/`, con Git ya instalado y configurado (Ejemplo 01).

## Comandos

```bash
cd mi-proyecto
git init

# Antes de crear ningun archivo
git status

# Crear un archivo nuevo
echo "# Mi proyecto" > README.md
git status

# Preparar el cambio (staging)
git add README.md
git status

# Confirmar el primer commit
git commit -m "Agregar README inicial"

# Segundo cambio: crear otro archivo
echo "console.log('hola');" > app.js
git add app.js
git commit -m "Agregar script inicial de la aplicacion"

# Ver el historial
git log --oneline
```

## Explicación paso a paso

1. `git init` crea el repositorio (una carpeta oculta `.git/`) en `mi-proyecto/`.
2. El primer `git status`, antes de crear ningún archivo, muestra "No commits yet" y
   que no hay nada para confirmar: el repositorio existe pero está vacío.
3. Al crear `README.md`, `git status` lo muestra como "Untracked" (no rastreado):
   existe en la carpeta, pero Git todavía no lo está siguiendo.
4. `git add README.md` lo mueve al área de staging; `git status` ahora lo muestra bajo
   "Changes to be committed" (listo para el próximo commit).
5. `git commit -m "..."` confirma ese cambio: queda como el primer commit del
   historial ("root-commit", porque es el primero).
6. Se repite el ciclo modificar → `git add` → `git commit` con un segundo archivo,
   `app.js`, y un segundo mensaje descriptivo.
7. `git log --oneline` muestra ambos commits, del más reciente al más antiguo, cada
   uno con un identificador corto y su mensaje.

## Resultado esperado

```console
$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)

$ echo "# Mi proyecto" > README.md
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
  README.md

$ git add README.md
$ git commit -m "Agregar README inicial"
[master (root-commit) df7472d] Agregar README inicial
 1 file changed, 1 insertion(+)
 create mode 100644 README.md

$ echo "console.log('hola');" > app.js
$ git add app.js
$ git commit -m "Agregar script inicial de la aplicacion"
[master 984b9cf] Agregar script inicial de la aplicacion
 1 file changed, 1 insertion(+)
 create mode 100644 app.js

$ git log --oneline
984b9cf Agregar script inicial de la aplicacion
df7472d Agregar README inicial
```

(Los identificadores de commit, por ejemplo `984b9cf`, son únicos para cada repositorio
y van a ser distintos en tu propia computadora; lo importante es que aparezcan dos
commits, en ese orden, con esos mensajes.)
