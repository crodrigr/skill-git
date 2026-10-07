# Ejemplo 03 — Merge con conflicto

## Estado inicial

Un repositorio con un commit en `master` (`README.md`). Dos ramas que, a partir de
ahí, modifican **la misma línea** de ese archivo de formas distintas.

## Comandos

```bash
git switch -c actualizar-bienvenida
echo "Bienvenido al mejor proyecto del mundo" > README.md
git add README.md
git commit -m "Mejorar el mensaje de bienvenida"

git switch master
echo "Bienvenido a nuestro proyecto de equipo" > README.md
git add README.md
git commit -m "Aclarar que es un proyecto de equipo"

git merge actualizar-bienvenida
git status
cat README.md

# resolver el conflicto a mano, dejando un unico contenido final:
echo "Bienvenido a nuestro mejor proyecto de equipo" > README.md
git add README.md
git status
git commit -m "Fusionar actualizar-bienvenida resolviendo conflicto en README"

git log --oneline --graph --all
cat README.md
```

## Explicación paso a paso

1. Ambas ramas modifican la misma línea de `README.md`, cada una con un contenido
   distinto: esto es exactamente lo que provoca un conflicto.
2. `git merge actualizar-bienvenida` falla ("Automatic merge failed"): Git no puede
   decidir solo cuál de las dos versiones es la correcta.
3. `git status` confirma el conflicto ("Unmerged paths") y `cat README.md` muestra el
   archivo con los marcadores de conflicto: entre `<<<<<<< HEAD` y `=======` está el
   contenido de la rama en la que estabas (`master`); entre `=======` y
   `>>>>>>> actualizar-bienvenida` está el contenido de la rama que se intentó
   fusionar.
4. Para resolver, se edita el archivo dejando el contenido final deseado (acá se
   combinaron ambos mensajes) y se **eliminan** los tres marcadores.
5. `git add README.md` marca el conflicto como resuelto; `git status` ahora dice "All
   conflicts fixed but you are still merging".
6. `git commit` (sin `-m` hace falta, pero se puede dar uno propio) completa el
   commit de fusión. El historial muestra las dos ramas uniéndose en ese commit.

## Resultado esperado

```console
$ git merge actualizar-bienvenida
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.

$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
  both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")

$ cat README.md
<<<<<<< HEAD
Bienvenido a nuestro proyecto de equipo
=======
Bienvenido al mejor proyecto del mundo
>>>>>>> actualizar-bienvenida

$ git add README.md
$ git status
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:
  modified:   README.md

$ git commit -m "Fusionar actualizar-bienvenida resolviendo conflicto en README"
[master 2fba3b3] Fusionar actualizar-bienvenida resolviendo conflicto en README

$ git log --oneline --graph --all
*   2fba3b3 Fusionar actualizar-bienvenida resolviendo conflicto en README
|\
| * 4c3f5f9 Mejorar el mensaje de bienvenida
* | 8175f0a Aclarar que es un proyecto de equipo
|/
* 7b5286e Agregar README inicial

$ cat README.md
Bienvenido a nuestro mejor proyecto de equipo
```

(Los identificadores de commit van a ser distintos en tu propio repositorio.)
