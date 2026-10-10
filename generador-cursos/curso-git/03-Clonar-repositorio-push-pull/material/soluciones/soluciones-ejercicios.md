# Soluciones — Ejercicios de la Clase 03 (material docente)

No compartir este archivo con los estudiantes.

Todos los comandos se reproducen tal cual en una terminal con Git, sin cuenta de GitHub
ni internet, usando repositorios `--bare` locales como remotos y la opción
`url.<base>.insteadOf` para que las URL `https://github.com/...` apunten a ellos (ver
`quickstart.md`, escenario 4). Las ediciones "a mano" de archivos se muestran con
`echo`, `sed` o `printf` para que sean reproducibles; en clase se hacen con un editor.

Los identificadores de commit varían entre ejecuciones. El aviso `You appear to have cloned an
empty repository` de la preparación es esperado (se clonan remotos vacíos antes de sembrarlos).

---

## Preparación común (docente)

Estado inicial de los ejercicios: el repositorio de ejemplo del docente, el repositorio
`diario` de la Clase 02 y dos copias de este último (A tuya, B de otra persona).

```bash
git config --global pull.rebase false

# Repositorio de ejemplo del docente (dos ramas)
git init -q --bare docente-git/proyecto-ejemplo.git
git clone -q docente-git/proyecto-ejemplo.git semilla
(cd semilla && echo "# Proyecto de ejemplo" > README.md && git add . \
  && git commit -qm "Crear README" && echo "Hola" > saludo.txt && git add . \
  && git commit -qm "Agregar saludo" && git push -q origin master \
  && git switch -qc mejoras && echo "Adios" >> saludo.txt \
  && git commit -qam "Agregar despedida" && git push -q origin mejoras)

# Repositorio diario de la Clase 02 y sus dos copias
git init -q --bare tu-usuario/diario.git
git clone -q https://github.com/tu-usuario/diario.git diario-A
(cd diario-A && printf 'Diario de estudio\nEntrada 1: instale Git\nEntrada 2: cree mi primera rama\n' > diario.txt \
  && git add . && git commit -qm "Crear diario con dos entradas" && git push -q origin master)
git clone -q https://github.com/tu-usuario/diario.git diario-B
```

---

## Básico 01 — Clonar un repositorio

### Comandos

```bash
git clone https://github.com/docente-git/proyecto-ejemplo.git
git clone https://github.com/docente-git/proyecto-ejemplo.git mi-ejemplo
cd mi-ejemplo
git remote -v
git log --oneline
git branch -a
cd ..
git clone https://github.com/docente-git/proyecto-ejemplo.git mi-ejemplo
```

### Explicación

El primer `clone` crea la carpeta `proyecto-ejemplo`; el segundo, `mi-ejemplo`. Dentro
de un clon, `git remote -v` muestra `origin` ya configurado (sin `git remote add`),
`git log --oneline` el historial completo y `git branch -a` las ramas remotas
(`origin/master`, `origin/mejoras`). El tercer `clone` falla porque `mi-ejemplo` ya
existe y no está vacía.

### Resultado esperado

```console
$ git remote -v
origin    https://github.com/docente-git/proyecto-ejemplo.git (fetch)
origin    https://github.com/docente-git/proyecto-ejemplo.git (push)
$ git log --oneline
<id> Agregar saludo
<id> Crear README
$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/mejoras
$ git clone https://github.com/docente-git/proyecto-ejemplo.git mi-ejemplo
fatal: destination path 'mi-ejemplo' already exists and is not an empty directory.
```

### Verificación

`git remote -v` apunta al repositorio de ejemplo, el historial tiene 2 commits y
aparecen las ramas remotas `master` y `mejoras`. Dos carpetas de clon existen.

---

## Básico 02 — Descargar cambios con pull

### Comandos

```bash
# Copia B: agrega y publica una entrada
cd diario-B
echo "Entrada 3: aprendi a clonar" >> diario.txt
git commit -am "Agregar entrada 3"
git push

# Copia A: aún no la tiene
cd ../diario-A
cat diario.txt
git pull
cat diario.txt
git log --oneline
git pull
```

### Explicación

Hasta que A hace pull, el archivo de A no tiene la entrada 3: el commit de B existe en
el remoto, no en A. `git pull` descarga y avanza (`Fast-forward`); un segundo pull
responde `Already up to date.`

### Resultado esperado

```console
$ git pull
Updating <id>..<id>
Fast-forward
 diario.txt | 1 +
 1 file changed, 1 insertion(+)
$ git log --oneline
<id> Agregar entrada 3
<id> Crear diario con dos entradas
$ git pull
Already up to date.
```

### Verificación

`diario.txt` de A contiene "Entrada 3: aprendi a clonar" tras el pull y no la contenía
antes.

---

## Intermedio 01 — Enviar cambios con push y resolver un rechazo

### Comandos

```bash
cd diario-A
# 1. commit y push en master
echo "Entrada 3: aprendi a clonar" >> diario.txt
git commit -am "Agregar entrada 3"
git push
# (en B: git pull confirma que lo recibe)
(cd ../diario-B && git pull)

# 2. rama nueva, primer push enlazado
git switch -c entrada-viaje
echo "Viaje: Git en el tren" > viaje.txt
git add viaje.txt
git commit -m "Agregar nota de viaje"
git push -u origin entrada-viaje
git branch -vv
git switch master

# 3. rechazo: B publica primero; A confirma otro archivo sin descargar
(cd ../diario-B && echo "Entrada 4: otra persona" >> diario.txt \
  && git commit -qam "Agregar entrada 4 (otra persona)" && git push -q)
echo "Resumen: clone y envie" > resumen.txt
git add resumen.txt
git commit -m "Agregar resumen"
git push

# 4. resolver: traer, y reintentar
git pull
git push

# 5. historial
git log --oneline --graph
```

### Explicación

El primer push publica el commit en `master`. La rama nueva necesita `-u` en su primer
envío para quedar enlazada con `origin/entrada-viaje`. En el paso 3, B publica antes;
el push de A es rechazado (`! [rejected] ... (fetch first)`) porque el remoto tiene un
commit que A no tiene. Como los cambios tocan archivos distintos, `git pull` crea un
commit de fusión sin conflicto y el siguiente `git push` se acepta. No se fuerza el
envío en ningún momento.

### Resultado esperado

```console
$ git push -u origin entrada-viaje
 * [new branch]      entrada-viaje -> entrada-viaje
Branch 'entrada-viaje' set up to track remote branch 'entrada-viaje' from 'origin'.
$ git push
 ! [rejected]        master -> master (fetch first)
$ git pull
Merge made by the 'ort' strategy.
$ git push
   <id>..<id>  master -> master
$ git log --oneline --graph
*   <id> Merge branch 'master' of https://github.com/tu-usuario/diario
|\
| * <id> Agregar entrada 4 (otra persona)
* | <id> Agregar resumen
|/
* <id> Agregar entrada 3
* <id> Crear diario con dos entradas
```

### Verificación

`git branch -vv` muestra `[origin/entrada-viaje]`; el rechazo ocurre en el paso 3; al
final, el historial remoto contiene los commits de A y de B (`git log --oneline
origin/master` en cualquiera de las copias).

---

## Intermedio 02 — Revertir un commit y propagarlo

### Comandos

```bash
cd diario-A
# 1. commit incorrecto, ya publicado
echo "Entrada 3: Git guarda los archivos en la nube" >> diario.txt
git commit -am "Agregar entrada 3 con dato incorrecto"
git push

# 2. localizar el commit
git log --oneline
H=$(git log --format=%h -1 --grep="dato incorrecto")

# 3. revertir
git revert --no-edit "$H"
git log --oneline
cat diario.txt

# 4. propagar
git push

# 5. copia B
cd ../diario-B
git pull
cat diario.txt
git log --oneline
```

### Explicación

`git revert` crea un commit nuevo que deshace el cambio y conserva el original. Al
publicarlo, el remoto queda corregido. B lo recibe con un `pull` de avance directo
(`Fast-forward`), sin conflictos, porque no se reescribió historial.

### Resultado esperado

```console
$ git revert --no-edit <id>
[master <id>] Revert "Agregar entrada 3 con dato incorrecto"
 1 file changed, 1 deletion(-)
$ git log --oneline
<id> Revert "Agregar entrada 3 con dato incorrecto"
<id> Agregar entrada 3 con dato incorrecto
<id> Crear diario con dos entradas
$ cat diario.txt
Diario de estudio
Entrada 1: instale Git
Entrada 2: cree mi primera rama
```

### Verificación

El historial de A y de B contiene el commit erróneo y el de reversión consecutivos; el
archivo no contiene "en la nube".

---

## Desafío 01 — Divergencia, conflicto y reversión

### Comandos

```bash
# 1. misma línea editada en A y en B
cd diario-A
sed -i '2s/.*/Entrada 1: instale Git (version tuya)/' diario.txt
git commit -am "Reescribir entrada 1 desde mi copia"
cd ../diario-B
sed -i '2s/.*/Entrada 1: instale Git y lo configure/' diario.txt
git commit -am "Ampliar entrada 1 desde otra copia"
git push
cd ../diario-A
git push                      # rechazado
git pull                      # conflicto en diario.txt
git status -sb
cat diario.txt                # marcadores de conflicto
# resolver: dejar una versión conjunta (equivale a editar a mano)
printf 'Diario de estudio\nEntrada 1: instale Git y lo configure (version conjunta)\nEntrada 2: cree mi primera rama\n' > diario.txt
git add diario.txt
git commit -m "Fusionar entrada 1 de ambas copias"
git push

# 2. dato incorrecto y un segundo commit sobre la misma línea
echo "Entrada 3: Git borra el historial al hacer push" >> diario.txt
git commit -am "Agregar entrada 3 con dato incorrecto"
sed -i '$s/.*/Entrada 3: Git borra el historial al hacer push (ampliado)/' diario.txt
git commit -am "Ampliar entrada 3"
git push

# 3. revertir el primero: conflicto
H=$(git log --format=%h -1 --grep="entrada 3 con dato")
git revert --no-edit "$H"
git status -sb
cat diario.txt
# resolver: quitar la línea incorrecta y su ampliación
printf 'Diario de estudio\nEntrada 1: instale Git y lo configure (version conjunta)\nEntrada 2: cree mi primera rama\n' > diario.txt
git add diario.txt
git revert --continue         # en clase: guardá y cerrá el editor
git log --oneline

# 4. propagar y comprobar en B
git push
cd ../diario-B
git pull
cat diario.txt

# 5. historial de B
git log --oneline
```

### Explicación

**Paso 1.** Ambas copias editaron la línea 2: el push de A es rechazado; el `pull`
termina en conflicto (`CONFLICT (content)`). Se resuelve como en la Clase 02: editar los
marcadores, `git add`, `git commit`; después el push se acepta.

**Pasos 2 y 3.** El segundo commit modificó la misma línea que el erróneo, así que
`git revert` no puede deshacer el primero solo y deja `diario.txt` en conflicto
(`UU` en `git status -sb`). Se resuelve dejando el archivo sin la línea incorrecta ni
su ampliación, `git add` y `git revert --continue`. Si se prefiere desistir:
`git revert --abort`.

**Pasos 4 y 5.** El push publica la reversión; B la recibe con `Fast-forward`.

### Resultado esperado

```console
$ git push
 ! [rejected]        master -> master (fetch first)
$ git pull
CONFLICT (content): Merge conflict in diario.txt
Automatic merge failed; fix conflicts and then commit the result.
$ cat diario.txt
Diario de estudio
<<<<<<< HEAD
Entrada 1: instale Git (version tuya)
=======
Entrada 1: instale Git y lo configure
>>>>>>> <id>
Entrada 2: cree mi primera rama
$ git revert --no-edit <id>
CONFLICT (content): Merge conflict in diario.txt
error: could not revert <id>... Agregar entrada 3 con dato incorrecto
$ git status -sb
## master...origin/master
UU diario.txt
$ git revert --continue
[master <id>] Revert "Agregar entrada 3 con dato incorrecto"
$ git log --oneline        # en B, tras el pull
<id> Revert "Agregar entrada 3 con dato incorrecto"
<id> Ampliar entrada 3
<id> Agregar entrada 3 con dato incorrecto
<id> Fusionar entrada 1 de ambas copias
$ cat diario.txt           # en B
Diario de estudio
Entrada 1: instale Git y lo configure (version conjunta)
Entrada 2: cree mi primera rama
```

### Verificación

Se cumplen los casos de prueba del Desafío: rechazo en el paso 1, conflicto en el pull,
commit de fusión en el remoto, conflicto al revertir (`UU`), reversión encima de los dos
commits del paso 2 en B, y `diario.txt` sin la línea incorrecta ni su ampliación.
