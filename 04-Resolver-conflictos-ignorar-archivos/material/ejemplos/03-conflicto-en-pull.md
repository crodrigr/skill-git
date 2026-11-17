# 📘 Ejemplo 03 — Conflicto al hacer pull, publicar y abortar

## 🧱 Estado inicial

Dos copias del repositorio `diario`, sincronizadas con el remoto en `master`: tu copia
(`diario-A`) y la de otra persona (`diario-B`). Se asume `pull.rebase false`. El remoto es
un repositorio `--bare` local en lugar de GitHub real (Git se comporta igual).

## 💻 Comandos

```bash
# "Otra persona" (copia B) cambia la línea 2 y la publica:
#   sed -i '2s/.*/Entrada 1: instale Git y lo configure/' diario.txt
#   git commit -am "Ampliar entrada 1 desde B"
#   git push

# Vos, en la copia A, cambiaste esa misma línea sin saberlo:
sed -i '2s/.*/Entrada 1: instale Git en mi equipo/' diario.txt
git commit -am "Reescribir entrada 1 desde A"

# El envío es rechazado; se trae el remoto y aparece el conflicto
git push
git pull
git status -sb
cat diario.txt

# Resolver combinando ambos lados, comprobar y completar la fusión
printf 'Diario de estudio\nEntrada 1: instale Git y lo configure en mi equipo\nEntrada 2: cree mi primera rama\nEntrada 3: aprendi a clonar\n' > diario.txt
grep -nE '^(<<<<<<<|=======|>>>>>>>)' diario.txt
git add diario.txt
git commit -m "Fusionar entrada 1 de A y B"

# Publicar la fusión
git push
git log --oneline --graph

# Variante: abortar en lugar de resolver.
# La copia B cambia la línea 3 y la publica; vos también la cambiaste en A y confirmaste:
#   sed -i '3s/.*/Entrada 2: cree mi rama/' diario.txt
#   git commit -am "Reescribir entrada 2 desde A"
git pull
git merge --abort
git status -sb
```

## 🔍 Explicación paso a paso

1. Ambas copias cambiaron la **misma línea** a partir del mismo commit. La copia B
   publica primero.
2. El `git push` de A es rechazado (`fetch first`): el remoto tiene un commit que A no
   tiene. Es lo mismo que viste en la Clase 03.
3. `git pull` descarga el commit de B y trata de fusionarlo con el de A. Como chocan en la
   línea 2, la fusión se detiene con `CONFLICT (content)`.
4. `git status -sb` muestra `UU diario.txt` y que tu rama está adelantada y atrasada
   respecto del remoto (`ahead 1, behind 1`). En los marcadores, `HEAD` es **tu** versión
   y la de abajo es la que vino del remoto.
5. Se resuelve combinando: "lo configuré" (de B) y "en mi equipo" (de A). El `grep` sin
   salida confirma que no quedan marcadores.
6. `git add` y `git commit` completan la fusión; `git push` la publica. Ahora el remoto
   tiene los cambios de ambos lados y el historial muestra el commit de fusión.
7. **Variante: abortar.** Si en cambio B publica un cambio en la línea 3 y vos también lo
   cambiaste en A, `git pull` volvería a detenerse en conflicto. Con `git merge --abort`
   el repositorio vuelve **exactamente** al estado previo al pull: tu commit sigue ahí, el
   archivo conserva tu versión y no se pierde nada. Después podés pensar el cambio y volver
   a hacer `git pull` cuando quieras.

## ✅ Resultado esperado

```console
$ git push
To https://github.com/tu-usuario/diario.git
 ! [rejected]        master -> master (fetch first)

$ git pull
From https://github.com/tu-usuario/diario
   8b46ddb..68478f1  master     -> origin/master
Auto-merging diario.txt
CONFLICT (content): Merge conflict in diario.txt
Automatic merge failed; fix conflicts and then commit the result.

$ git status -sb
## master...origin/master [ahead 1, behind 1]
UU diario.txt

$ cat diario.txt
Diario de estudio
<<<<<<< HEAD
Entrada 1: instale Git en mi equipo
=======
Entrada 1: instale Git y lo configure
>>>>>>> 68478f1a063b23b0ddabf88442203dfdd59bc20d
Entrada 2: cree mi primera rama
Entrada 3: aprendi a clonar

$ grep -nE '^(<<<<<<<|=======|>>>>>>>)' diario.txt
$ git commit -m "Fusionar entrada 1 de A y B"
[master 37d85a7] Fusionar entrada 1 de A y B

$ git push
To https://github.com/tu-usuario/diario.git
   68478f1..37d85a7  master -> master

$ git log --oneline --graph
*   37d85a7 Fusionar entrada 1 de A y B
|\
| * 68478f1 Ampliar entrada 1 desde B
* | 30dd874 Reescribir entrada 1 desde A
|/

$ git pull
Auto-merging diario.txt
CONFLICT (content): Merge conflict in diario.txt
Automatic merge failed; fix conflicts and then commit the result.

$ git merge --abort
$ git status -sb
## master...origin/master [ahead 1, behind 1]
```

> 💡 Los identificadores serán distintos en tu repositorio. En el rechazo del `push` se
> omitieron las líneas de ayuda `hint:` que ya conocés de la Clase 03. Tras abortar, el
> archivo `diario.txt` contiene otra vez tu versión y `ahead 1, behind 1` indica que
> seguís con un commit propio que aún no se integró.
