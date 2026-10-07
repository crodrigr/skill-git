# Ejemplo 01 — Gestión básica de ramas

## Estado inicial

Un repositorio local con un commit en la rama principal (`master`), con un archivo
`notas.txt`.

## Comandos

```bash
git branch

git switch -c funcionalidad-nueva
git branch

echo "Idea para la funcionalidad nueva" >> notas.txt
git add notas.txt
git commit -m "Agregar idea de la funcionalidad nueva"

git switch master
cat notas.txt

git merge funcionalidad-nueva -m "Fusionar funcionalidad-nueva en master"
cat notas.txt

git branch -d funcionalidad-nueva
git branch
```

## Explicación paso a paso

1. `git branch` (sin argumentos) lista las ramas existentes; al principio solo
   existe `master`, marcada con `*` porque es donde estás parado.
2. `git switch -c funcionalidad-nueva` crea una rama nueva y cambia a ella en un solo
   paso; `git branch` ahora muestra dos ramas, con `*` en la nueva.
3. El commit que se hace a partir de acá queda **solo** en `funcionalidad-nueva`.
4. `git switch master` vuelve a la rama principal: `cat notas.txt` muestra que el
   cambio de la otra rama **no** está ahí todavía.
5. `git merge funcionalidad-nueva` incorpora esos commits a `master`. Como `master`
   no tenía commits propios desde que se creó la rama, Git hace un **fast-forward**:
   simplemente mueve el puntero de `master`, sin crear un commit de fusión nuevo (por
   eso el mensaje dice que el `-m` se ignora). El archivo ahora sí tiene el cambio.
6. `git branch -d funcionalidad-nueva` elimina la rama sin advertencias, porque ya
   está completamente fusionada: no hay riesgo de perder commits.

## Resultado esperado

```console
$ git branch
* master

$ git switch -c funcionalidad-nueva
Switched to a new branch 'funcionalidad-nueva'
$ git branch
* funcionalidad-nueva
  master

$ git commit -m "Agregar idea de la funcionalidad nueva"
[funcionalidad-nueva d47d5e2] Agregar idea de la funcionalidad nueva
 1 file changed, 1 insertion(+)

$ git switch master
Switched to branch 'master'
$ cat notas.txt
Version inicial

$ git merge funcionalidad-nueva -m "Fusionar funcionalidad-nueva en master"
Updating bf12616..d47d5e2
Fast-forward (no commit created; -m option ignored)
 notas.txt | 1 +
 1 file changed, 1 insertion(+)
$ cat notas.txt
Version inicial
Idea para la funcionalidad nueva

$ git branch -d funcionalidad-nueva
Deleted branch funcionalidad-nueva (was d47d5e2).
$ git branch
* master
```

(Los identificadores de commit van a ser distintos en tu propio repositorio.)
