# Ejemplo 04 — Conflicto al recuperar cambios guardados y al revertir

## Estado inicial

Tu copia del repositorio `diario` (`diario-A`), sincronizada con el remoto en `master`,
y la copia de otra persona (`diario-B`). Se asume `pull.rebase false`. El remoto es un
repositorio `--bare` local en lugar de GitHub real.

La otra persona ya publicó un cambio en la línea 2 de `diario.txt`. Vos, sin saberlo,
editaste esa misma línea y todavía **no la confirmaste**.

## Comandos

```bash
# 1. Conflicto con stash pop
git status -s
git stash                      # guardar tu edición sin confirmar
git pull                       # traer lo de la otra persona (avance directo)
git stash pop                  # devolver tu edición: choca con la línea 2
git status -s
cat diario.txt
git stash list

# Resolver: combinar ambos lados, marcar como resuelto y descartar el stash
printf 'Diario de estudio\nEntrada 1: instale Git y lo configure en mi equipo\nEntrada 2: cree mi primera rama\nEntrada 3: aprendi a clonar\n' > diario.txt
git add diario.txt
git stash drop
git commit -m "Combinar la entrada 1 de ambos lados"

# 2. Conflicto con revert: dos commits seguidos sobre la misma línea
echo "Entrada 4: Git borra el historial al hacer push" >> diario.txt
git commit -am "Agregar entrada 4 con dato incorrecto"
sed -i '$s/.*/Entrada 4: Git borra el historial al hacer push (ampliado)/' diario.txt
git commit -am "Ampliar entrada 4"
git push

# Revertir el primero (el erróneo): choca con el segundo
H=$(git log --format=%h -1 --grep="entrada 4 con dato")
git revert --no-edit "$H"
git status -sb
cat diario.txt

# Resolver: quitar la línea incorrecta y su ampliación, y continuar
printf 'Diario de estudio\nEntrada 1: instale Git y lo configure en mi equipo\nEntrada 2: cree mi primera rama\nEntrada 3: aprendi a clonar\n' > diario.txt
git add diario.txt
git revert --continue
git log --oneline
```

## Explicación paso a paso

1. Con cambios sin confirmar en un archivo que el remoto también modificó, `git pull`
   se negaría (Clase 03). Por eso `git stash` los guarda primero y `git pull` trae lo
   nuevo sin problemas.
2. `git stash pop` vuelve a aplicar tu edición sobre el estado actualizado. Como ambos
   lados cambiaron la línea 2, aparece `CONFLICT (content)`. Los marcadores dicen
   `Updated upstream` (lo que vino del remoto) y `Stashed changes` (tu edición guardada).
3. Git avisa `The stash entry is kept in case you need it again`: **el stash no se
   descarta** si hubo conflicto. `git stash list` lo confirma.
4. Se resuelve como cualquier conflicto: se edita el archivo, `git add` lo marca como
   resuelto y recién entonces `git stash drop` descarta el stash que ya no hace falta.
5. Para el revert, se crean dos commits que tocan la misma línea del final y se publican.
   Después se revierte el **primero**: Git no puede deshacerlo solo porque el segundo ya
   cambió esa línea.
6. `git revert` se detiene con `CONFLICT (content)` y `git status -sb` muestra
   `UU diario.txt`. El marcador deja a la vista la línea ampliada de un lado y vacío del
   otro: la reversión quiere borrar la entrada.
7. Se resuelve dejando el archivo sin la entrada, `git add` y `git revert --continue`,
   que crea el commit de reversión. Si preferís desistir: `git revert --abort`.

## Resultado esperado

```console
$ git status -s
 M diario.txt

$ git stash
Saved working directory and index state WIP on master: e72015a Crear diario, configuracion y notas

$ git pull
From https://github.com/tu-usuario/diario
   e72015a..40c8639  master     -> origin/master
Updating e72015a..40c8639
Fast-forward
 diario.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)

$ git stash pop
Auto-merging diario.txt
CONFLICT (content): Merge conflict in diario.txt
On branch master
Your branch is up to date with 'origin/master'.

Unmerged paths:
    both modified:   diario.txt

The stash entry is kept in case you need it again.

$ git status -s
UU diario.txt

$ cat diario.txt
Diario de estudio
<<<<<<< Updated upstream
Entrada 1: instale Git y lo configure
=======
Entrada 1: instale Git en mi equipo
>>>>>>> Stashed changes
Entrada 2: cree mi primera rama
Entrada 3: aprendi a clonar

$ git stash list
stash@{0}: WIP on master: e72015a Crear diario, configuracion y notas

$ git stash drop
Dropped refs/stash@{0} (5b5b1ee1ba38d01c6b971136e5a1735b99b41399)

$ git revert --no-edit f15daab
Auto-merging diario.txt
CONFLICT (content): Merge conflict in diario.txt
error: could not revert f15daab... Agregar entrada 4 con dato incorrecto

$ git status -sb
## master...origin/master
UU diario.txt

$ cat diario.txt
Diario de estudio
Entrada 1: instale Git y lo configure en mi equipo
Entrada 2: cree mi primera rama
Entrada 3: aprendi a clonar
<<<<<<< HEAD
Entrada 4: Git borra el historial al hacer push (ampliado)
=======
>>>>>>> parent of f15daab (Agregar entrada 4 con dato incorrecto)

$ git revert --continue
[master 1046c9e] Revert "Agregar entrada 4 con dato incorrecto"
 1 file changed, 1 deletion(-)

$ git log --oneline
1046c9e Revert "Agregar entrada 4 con dato incorrecto"
50ae52b Ampliar entrada 4
f15daab Agregar entrada 4 con dato incorrecto
e845d4c Combinar la entrada 1 de ambos lados
```

(Los identificadores serán distintos en tu repositorio. Se omitieron las líneas de ayuda
`hint:` y de ayuda de `git status` que no cambian el significado. Si el editor se abre al
ejecutar `git revert --continue`, guardá y cerrá.)
