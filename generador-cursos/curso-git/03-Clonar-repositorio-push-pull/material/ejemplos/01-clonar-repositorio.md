# Ejemplo 01 — Clonar un repositorio

## Estado inicial

Existe un repositorio remoto de ejemplo, público, provisto por el docente
(`proyecto-ejemplo`), con dos ramas (`master` y `mejoras`). Tu máquina no tiene ninguna
copia. En este ejemplo el remoto es un repositorio `--bare` local en lugar de GitHub
real: Git se comporta igual.

## Comandos

```bash
# Preparación: integrar los cambios del remoto mediante fusión al hacer pull
git config --global pull.rebase false
git config --get pull.rebase

# Clonar con la carpeta por defecto
git clone https://github.com/docente-git/proyecto-ejemplo.git

# Clonar eligiendo el nombre de la carpeta
git clone https://github.com/docente-git/proyecto-ejemplo.git mi-ejemplo

# Verificar la copia
cd mi-ejemplo
git remote -v
git log --oneline
git branch -a
git status

# Intentar clonar otra vez en una carpeta que ya existe y tiene contenido
cd ..
git clone https://github.com/docente-git/proyecto-ejemplo.git mi-ejemplo
```

## Explicación paso a paso

1. `git config --global pull.rebase false` fija cómo se integrarán los cambios al
   hacer `git pull` (por fusión). `git config --get pull.rebase` confirma el valor.
   Solo se hace una vez por equipo.
2. `git clone <url>` crea una carpeta con el nombre del repositorio
   (`proyecto-ejemplo`) y descarga archivos, historial y ramas.
3. `git clone <url> mi-ejemplo` hace lo mismo, pero la carpeta se llama `mi-ejemplo`.
4. `git remote -v` muestra que `origin` ya apunta al repositorio clonado: no hubo que
   ejecutar `git remote add`.
5. `git log --oneline` muestra el historial completo que llegó con el clon.
6. `git branch -a` lista la rama local `master` y las ramas remotas
   (`remotes/origin/...`), incluida `mejoras`, que aún no tiene copia local.
7. `git status` confirma que la copia está limpia y alineada con `origin/master`.
8. Al clonar en una carpeta que ya existe y no está vacía, Git se niega: elegí otro
   nombre o ubicación.

## Resultado esperado

```console
$ git config --global pull.rebase false
$ git config --get pull.rebase
false

$ git clone https://github.com/docente-git/proyecto-ejemplo.git
Cloning into 'proyecto-ejemplo'...
done.

$ git clone https://github.com/docente-git/proyecto-ejemplo.git mi-ejemplo
Cloning into 'mi-ejemplo'...
done.

$ cd mi-ejemplo
$ git remote -v
origin    https://github.com/docente-git/proyecto-ejemplo.git (fetch)
origin    https://github.com/docente-git/proyecto-ejemplo.git (push)

$ git log --oneline
21c625d Agregar saludo
f540d5f Crear README

$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/mejoras

$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean

$ cd ..
$ git clone https://github.com/docente-git/proyecto-ejemplo.git mi-ejemplo
fatal: destination path 'mi-ejemplo' already exists and is not an empty directory.
```

(Los identificadores de commit serán distintos en tu propio repositorio. Con un remoto
real verás además líneas de progreso como `remote: Enumerating objects...`. Si el
repositorio usa `main` como rama principal, verás `main` donde aquí aparece `master`.)
