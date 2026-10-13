# Ejemplo 01 — Conflicto en varios archivos

## Estado inicial

Tu copia del repositorio `diario` (Clase 03), en `master`, con cuatro archivos
rastreados: `diario.txt`, `configuracion.txt`, `notas.txt` y `leeme.txt`. Existe una rama
`reorganizar` que partió del mismo commit y que:

- cambió la línea 2 de `diario.txt` y la línea 2 de `configuracion.txt`,
- **eliminó** `notas.txt`,
- cambió `leeme.txt`.

Mientras tanto, `master` también cambió la línea 2 de `diario.txt` y de
`configuracion.txt`, y **modificó** `notas.txt`, pero no tocó `leeme.txt`.

## Comandos

```bash
# Fusionar la rama en master: Git se detiene con conflictos
git merge reorganizar

# Identificar qué archivos están en conflicto
git status
git status -s

# Ver los marcadores
cat diario.txt
cat configuracion.txt

# Resolver diario.txt y configuracion.txt (equivale a editar a mano):
printf 'Diario de estudio\nEntrada 1: instale Git y lo configure en mi equipo\nEntrada 2: cree mi primera rama\nEntrada 3: aprendi a clonar\n' > diario.txt
printf 'idioma=es\ntema=oscuro\nfuente=normal\n' > configuracion.txt

# notas.txt: decidir. Aquí se acepta la eliminación
git rm notas.txt

# Marcar los dos archivos editados como resueltos y revisar el estado
git add diario.txt configuracion.txt
git status -s

# Completar la fusión
git commit -m "Fusionar reorganizar con master"
git log --oneline --graph
```

## Explicación paso a paso

1. `git merge reorganizar` fusiona solo lo que no choca: `leeme.txt` (cambiado únicamente
   en la rama) se integra solo. Los otros tres archivos quedan en conflicto y la fusión
   se detiene.
2. `git status` separa dos grupos. Bajo `Changes to be committed` queda `leeme.txt`, ya
   fusionado. Bajo `Unmerged paths` están los archivos que necesitan tu decisión:
   `both modified` para `configuracion.txt` y `diario.txt`, y `deleted by them` para
   `notas.txt`.
3. `git status -s` lo resume con dos letras: `UU` (ambos modificaron) y `UD` (nosotros
   modificamos, ellos eliminaron).
4. En `diario.txt` y `configuracion.txt` hay un bloque con marcadores. En el diario se
   **combinan** ambos lados (la entrada recoge el "lo configuré" de la rama y el "en mi
   equipo" de `master`); en la configuración se **conserva el lado entrante**
   (`tema=oscuro`).
5. `notas.txt` es el caso de modificado y eliminado: Git deja tu versión en la carpeta,
   pero te pide decidir. Se puede conservar (`git add notas.txt`) o eliminar
   (`git rm notas.txt`). Aquí se elimina, como hizo la rama.
6. `git add` marca cada archivo como resuelto. En `git status -s` pasan a `M` en la primera columna
   (listos para confirmar), y `notas.txt` a `D`.
7. `git commit` crea el commit de fusión. `git log --graph` muestra las dos líneas que se
   juntan.

## Resultado esperado

```console
$ git merge reorganizar
Auto-merging configuracion.txt
CONFLICT (content): Merge conflict in configuracion.txt
Auto-merging diario.txt
CONFLICT (content): Merge conflict in diario.txt
CONFLICT (modify/delete): notas.txt deleted in reorganizar and modified in HEAD.  Version HEAD of notas.txt left in tree.
Automatic merge failed; fix conflicts and then commit the result.

$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Changes to be committed:
    modified:   leeme.txt

Unmerged paths:
  (use "git add/rm <file>..." as appropriate to mark resolution)
    both modified:   configuracion.txt
    both modified:   diario.txt
    deleted by them: notas.txt

$ git status -s
UU configuracion.txt
UU diario.txt
M  leeme.txt
UD notas.txt

$ cat diario.txt
Diario de estudio
<<<<<<< HEAD
Entrada 1: instale Git en mi equipo
=======
Entrada 1: instale Git y lo configure
>>>>>>> reorganizar
Entrada 2: cree mi primera rama
Entrada 3: aprendi a clonar

$ cat configuracion.txt
idioma=es
<<<<<<< HEAD
tema=azul
=======
tema=oscuro
>>>>>>> reorganizar
fuente=normal

$ git status -s
M  configuracion.txt
M  diario.txt
M  leeme.txt
D  notas.txt

$ git commit -m "Fusionar reorganizar con master"
[master d700fbb] Fusionar reorganizar con master

$ git log --oneline --graph
*   d700fbb Fusionar reorganizar con master
|\
| * 7953638 Reorganizar diario, configuracion y notas
* | 841d36b Ajustar diario, configuracion y notas en master
|/
* e5db1f0 Crear diario, configuracion y notas
```

(Los identificadores de commit serán distintos en tu repositorio. En el `git status`
completo se omitieron algunas líneas de ayuda que no cambian el significado.)
