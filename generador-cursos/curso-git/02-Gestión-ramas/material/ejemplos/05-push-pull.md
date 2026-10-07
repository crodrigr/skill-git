# Ejemplo 05 — Publicar y traer cambios

## Estado inicial

Dos copias del mismo proyecto conectadas al mismo remoto (en este ejemplo, un
repositorio `--bare` local, en lugar de GitHub real — ver Ejemplo 04): tu repositorio
(`ej05-local`) y uno que simula a otra persona colaborando (`ej05-otra-persona`,
clonado del mismo remoto).

## Comandos

```bash
# En tu repositorio local, primera publicacion:
git push -u origin master

# "Otra persona" clona el remoto, hace un cambio y lo publica:
#   (en su propia copia)
#   echo "Segunda entrada del diario" >> diario.txt
#   git add diario.txt && git commit -m "Agregar segunda entrada al diario"
#   git push origin master

# Mientras tanto, en tu copia, haces otro cambio sin saber del anterior:
echo "Entrada distinta desde el otro lado" >> diario.txt
git add diario.txt
git commit -m "Agregar otra entrada desde el local original"

# Intentas publicar: Git lo rechaza
git push origin master

# Traes los cambios del remoto primero:
git pull --no-rebase origin master

# Si hay conflicto (como en este caso, ambas ediciones tocaron el mismo lugar del
# archivo), se resuelve igual que en el Ejemplo 03:
cat diario.txt   # ver los marcadores de conflicto
# editar el archivo a mano, dejando el contenido final:
git add diario.txt
git commit -m "Fusionar cambios del remoto con la entrada local"

# Ahora si, publicar:
git push origin master
```

## Explicación paso a paso

1. El primer `git push -u origin master` publica el historial inicial y deja
   configurado que tu rama local `master` sigue a `origin/master` (el `-u` solo hace
   falta la primera vez).
2. "Otra persona" (en este ejemplo, un segundo clon del mismo remoto) agrega un
   commit propio y lo publica. Ahora el remoto tiene un commit que tu copia local no
   tiene.
3. Vos, sin saberlo, agregás otro commit en tu copia. Al intentar `git push`, Git lo
   **rechaza**: el remoto avanzó con un commit que vos no tenés, y Git no te deja
   sobrescribirlo a ciegas.
4. `git pull --no-rebase origin master` trae ese commit y lo combina con el tuyo. En
   este caso, como ambos cambios modificaron la misma parte del archivo, el `pull`
   termina en un **conflicto** (el mismo tipo de conflicto del Ejemplo 03, solo que
   uno de los dos lados vino del remoto).
5. Se resuelve exactamente igual: editar el archivo quitando los marcadores,
   `git add`, y `git commit` para completar la fusión que trajo el `pull`.
6. Recién ahora `git push` se completa sin problemas: el historial local ya
   incorpora el commit que faltaba.

## Resultado esperado

```console
$ git push -u origin master
To .../remoto-ej05.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.

$ git push origin master
To .../remoto-ej05.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to '.../remoto-ej05.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.

$ git pull --no-rebase origin master
From .../remoto-ej05
 * branch            master     -> FETCH_HEAD
Auto-merging diario.txt
CONFLICT (content): Merge conflict in diario.txt
Automatic merge failed; fix conflicts and then commit the result.

$ cat diario.txt
Primera version
<<<<<<< HEAD
Entrada distinta desde el otro lado
=======
Segunda entrada del diario
>>>>>>> d0e432e

$ git add diario.txt
$ git commit -m "Fusionar cambios del remoto con la entrada local"
[master afaca4a] Fusionar cambios del remoto con la entrada local

$ git push origin master
To .../remoto-ej05.git
   d0e432e..afaca4a  master -> master

$ cat diario.txt
Primera version
Segunda entrada del diario
Entrada distinta desde el otro lado
```

(Las rutas `.../remoto-ej05.git` y los identificadores de commit van a ser distintos
en tu propio repositorio; en GitHub real, la ruta sería una URL `https://github.com/...`.)
