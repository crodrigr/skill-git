# Ejemplo 02 — Pull: con novedades, sin novedades y con cambios locales

## Estado inicial

Dos copias del repositorio `diario` de la Clase 02, ambas sincronizadas con el remoto:
tu copia (`diario`) y una que simula a otra persona (`otra-persona`, clonada del mismo
remoto). El archivo `diario.txt` tiene tres líneas: el título y dos entradas. Se asume
`pull.rebase false` (Ejemplo 01). El remoto es un repositorio `--bare` local, igual que
en la Clase 02.

## Comandos

```bash
# En la copia "otra-persona": agrega una entrada y la publica
#   echo "Entrada 3: aprendi a clonar" >> diario.txt
#   git commit -am "Agregar entrada 3"
#   git push

# En tu copia: traer las novedades
cd diario
git pull
cat diario.txt
git log --oneline

# Volver a hacer pull cuando no hay nada nuevo
git pull

# "Otra persona" publica una entrada 4; vos, mientras tanto, editás el título
# sin confirmar:
sed -i '1s/.*/Diario de estudio de Git/' diario.txt     # edición local sin commit
git pull                                                # Git se niega

# Guardar temporalmente, traer, recuperar
git stash
git pull
git stash pop
cat diario.txt
```

## Explicación paso a paso

1. La otra persona publicó un commit que tu copia no tiene. `git pull` lo descarga
   (`From ...`) y, como vos no tenías commits propios, lo integra con un avance directo
   (`Fast-forward`).
2. `cat diario.txt` y `git log --oneline` muestran la entrada 3 y su commit.
3. Un segundo `git pull` sin novedades responde `Already up to date.` No es un error.
4. Ahora el remoto avanza otra vez (entrada 4) y vos tenés un cambio **sin confirmar**
   en `diario.txt`, el mismo archivo que el pull va a modificar. Git aborta con
   `Your local changes ... would be overwritten by merge` para no perder tu edición.
5. `git stash` guarda tus cambios sin confirmar y deja el árbol limpio. Ahora
   `git pull` funciona.
6. `git stash pop` devuelve tus cambios sobre la versión actualizada. Como tocaste
   líneas distintas, se combinan solos. (La otra salida válida habría sido confirmar
   tu cambio antes de hacer pull.)

## Resultado esperado

```console
$ git pull
From https://github.com/tu-usuario/diario
   5839ba6..cc77773  master     -> origin/master
Updating 5839ba6..cc77773
Fast-forward
 diario.txt | 1 +
 1 file changed, 1 insertion(+)

$ cat diario.txt
Diario de estudio
Entrada 1: instale Git
Entrada 2: cree mi primera rama
Entrada 3: aprendi a clonar

$ git log --oneline
cc77773 Agregar entrada 3
5839ba6 Crear diario con dos entradas

$ git pull
Already up to date.

$ git pull
From https://github.com/tu-usuario/diario
   cc77773..654d3f6  master     -> origin/master
error: Your local changes to the following files would be overwritten by merge:
    diario.txt
Please commit your changes or stash them before you merge.
Aborting
Updating cc77773..654d3f6

$ git stash
Saved working directory and index state WIP on master: cc77773 Agregar entrada 3

$ git pull
Updating cc77773..654d3f6
Fast-forward
 diario.txt | 1 +
 1 file changed, 1 insertion(+)

$ git stash pop
Auto-merging diario.txt
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
    modified:   diario.txt

Dropped refs/stash@{0} (8a86c2d336ed06edce1502cbd17e46202463ac7c)

$ cat diario.txt
Diario de estudio de Git
Entrada 1: instale Git
Entrada 2: cree mi primera rama
Entrada 3: aprendi a clonar
Entrada 4: aprendi pull
```

(Los identificadores de commit serán distintos en tu repositorio. En el resultado se
omitieron líneas de ayuda de Git que no cambian el significado.)
