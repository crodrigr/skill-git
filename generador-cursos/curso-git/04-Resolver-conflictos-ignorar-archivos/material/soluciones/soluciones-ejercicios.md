# Soluciones — Ejercicios de la Clase 04 (material docente)

No compartir este archivo con los estudiantes.

Todos los comandos se reproducen tal cual en una terminal con Git, sin cuenta de GitHub
ni internet, usando repositorios `--bare` locales como remotos y la opción
`url.<base>.insteadOf` para que las URL `https://github.com/...` apunten a ellos (ver
`quickstart.md`, escenario 4). Las ediciones "a mano" de archivos se muestran con
`echo`, `sed` o `printf` para que sean reproducibles; en clase se hacen con un editor.
Las credenciales de los ejemplos son ficticias (**NO REAL**).

Los identificadores de commit varían entre ejecuciones. El aviso `You appear to have
cloned an empty repository` de la preparación es esperado (se clonan remotos vacíos antes
de sembrarlos).

---

## Preparación común (docente)

Estado inicial de los ejercicios con remoto: el repositorio `diario` y dos copias (A
tuya, B de otra persona), con `diario.txt`, `configuracion.txt`, `notas.txt` y `leeme.txt`.
Cada ejercicio añade su propia preparación específica.

```bash
git config --global pull.rebase false

git init -q --bare tu-usuario/diario.git
git clone -q https://github.com/tu-usuario/diario.git diario-A
(cd diario-A \
  && printf 'Diario de estudio\nEntrada 1: instale Git\nEntrada 2: cree mi primera rama\nEntrada 3: aprendi a clonar\n' > diario.txt \
  && printf 'idioma=es\ntema=claro\nfuente=normal\n' > configuracion.txt \
  && git add . && git commit -qm "Crear diario y configuracion" && git push -q origin master)
git clone -q https://github.com/tu-usuario/diario.git diario-B
```

---

## Básico 01 — Resolver un conflicto en dos archivos

### Preparación del ejercicio

```bash
cd diario-A
git switch -qc ajustes
sed -i '2s/.*/Entrada 1: instale Git y lo configure/' diario.txt
sed -i '2s/.*/tema=oscuro/' configuracion.txt
git commit -qam "Ajustar diario y configuracion en ajustes"
git switch -q master
sed -i '2s/.*/Entrada 1: instale Git en mi equipo/' diario.txt
sed -i '2s/.*/tema=azul/' configuracion.txt
git commit -qam "Ajustar diario y configuracion en master"
```

### Comandos

```bash
git merge ajustes
git status -s
# diario.txt: combinar ambos lados; configuracion.txt: lado entrante
printf 'Diario de estudio\nEntrada 1: instale Git y lo configure en mi equipo\nEntrada 2: cree mi primera rama\nEntrada 3: aprendi a clonar\n' > diario.txt
printf 'idioma=es\ntema=oscuro\nfuente=normal\n' > configuracion.txt
grep -rnE '^(<<<<<<<|=======|>>>>>>>)' diario.txt configuracion.txt
git diff --check
git add diario.txt configuracion.txt
git commit -m "Fusionar ajustes con master"
git status -s
git log --oneline --graph
```

### Explicación

La fusión se detiene con dos conflictos de contenido. `git status -s` los marca `UU`; como
no hay archivos en `Changes to be committed`, no se fusionó nada solo. En `diario.txt`
se combinan ambos lados y en `configuracion.txt` se queda el lado entrante. El `grep` y
`git diff --check` sin salida confirman que no quedan marcadores. `git add` marca la
resolución y `git commit` completa la fusión.

### Resultado esperado

```console
$ git status -s
UU configuracion.txt
UU diario.txt
$ git commit -m "Fusionar ajustes con master"
[master <id>] Fusionar ajustes con master
$ git status -s
$ git log --oneline --graph
*   <id> Fusionar ajustes con master
|\
| * <id> Ajustar diario y configuracion en ajustes
* | <id> Ajustar diario y configuracion en master
|/
* <id> Crear diario y configuracion
```

### Verificación

Sin marcadores en ambos archivos, `git status` limpio, commit de fusión con dos padres.

---

## Básico 02 — Crear un .gitignore

### Preparación del ejercicio

```bash
mkdir -p proyecto-gitignore/{src,build,dependencias}
cd proyecto-gitignore && git init -q
echo "# Proyecto de practica" > README.md
echo "programa de ejemplo" > src/programa.txt
echo "salida generada" > build/salida.txt
echo "registro" > registro.log
echo "depuracion" > depuracion.log
echo "importante: no borrar" > auditoria.log
echo "libreria de ejemplo" > dependencias/libreria.txt
printf 'usuario=demo\nclave=NO-REAL-12345\n' > credenciales.txt
```

### Comandos

```bash
git status -s
cat > .gitignore <<'G'
# Carpeta de salida generada
build/

# Logs, excepto auditoria
*.log
!auditoria.log

# Dependencias
dependencias/

# Secretos (credenciales NO REAL en este curso)
credenciales.txt
G
git status -s
git add .
git commit -m "Crear proyecto con .gitignore"
git ls-files
```

### Explicación

`build/` ignora la carpeta; `*.log` ignora por comodín y `!auditoria.log` es la excepción,
que debe ir **después**; `dependencias/` ignora la carpeta; `credenciales.txt` ignora por
nombre exacto. Tras crear el archivo, solo quedan `.gitignore`, `README.md`,
`auditoria.log` y `src/` como no rastreados.

### Resultado esperado

```console
$ git status -s
?? .gitignore
?? README.md
?? auditoria.log
?? src/
$ git ls-files
.gitignore
README.md
auditoria.log
src/programa.txt
```

### Verificación

`git ls-files` no incluye `build/`, `dependencias/`, `registro.log`, `depuracion.log` ni
`credenciales.txt`; sí incluye `auditoria.log`.

---

## Intermedio 01 — Conflicto al hacer pull: resolver, publicar y abortar

### Comandos

```bash
# 1. Conflicto: misma línea en B y en A
cd diario-B
sed -i '2s/.*/Entrada 1: instale Git y lo configure/' diario.txt
git commit -qam "Ampliar entrada 1 desde B"
git push -q
cd ../diario-A
sed -i '2s/.*/Entrada 1: instale Git en mi equipo/' diario.txt
git commit -qam "Reescribir entrada 1 desde A"

# 2. El envío es rechazado
git push

# 3. Traer y examinar el conflicto
git pull
git status -sb
cat diario.txt

# 4. Resolver, comprobar, completar y publicar
printf 'Diario de estudio\nEntrada 1: instale Git y lo configure en mi equipo\nEntrada 2: cree mi primera rama\nEntrada 3: aprendi a clonar\n' > diario.txt
grep -nE '^(<<<<<<<|=======|>>>>>>>)' diario.txt
git diff --check
git add diario.txt
git commit -m "Fusionar entrada 1 de A y B"
git push

# 5. Segundo conflicto: línea 3, y abortar
cd ../diario-B
git pull -q
sed -i '3s/.*/Entrada 2: cree mi primera rama de trabajo/' diario.txt
git commit -qam "Ampliar entrada 2 desde B"
git push -q
cd ../diario-A
sed -i '3s/.*/Entrada 2: cree mi rama/' diario.txt
git commit -qam "Reescribir entrada 2 desde A"
git pull
git status -sb
git merge --abort

# 6. Verificar
git status -sb
cat diario.txt
git log --oneline | head -3
```

### Explicación

Ambas copias cambiaron la misma línea desde el mismo commit, así que el push de A es
rechazado y el pull termina en conflicto (`ahead 1, behind 1` indica un commit propio sin
publicar y uno remoto sin traer). Se resuelve combinando, se comprueba con `grep` y
`git diff --check`, y se publica. En el segundo conflicto, `git merge --abort` devuelve
el repositorio al estado previo al pull, con el commit propio intacto y sin marcadores.

### Resultado esperado

```console
$ git push
 ! [rejected]        master -> master (fetch first)
$ git pull
CONFLICT (content): Merge conflict in diario.txt
$ git status -sb
## master...origin/master [ahead 1, behind 1]
UU diario.txt
$ git push
   <id>..<id>  master -> master
$ git merge --abort
$ git status -sb
## master...origin/master [ahead 1, behind 1]
$ cat diario.txt
Diario de estudio
Entrada 1: instale Git y lo configure en mi equipo
Entrada 2: cree mi rama
Entrada 3: aprendi a clonar
```

### Verificación

Tras el paso 4, el remoto contiene los commits de A y B y el de fusión. Tras el paso 6,
no hay fusión en curso, `diario.txt` conserva la versión de A y el commit sigue en el
historial.

---

## Intermedio 02 — Dejar de rastrear un archivo y averiguar qué regla lo ignora

### Preparación del ejercicio

```bash
mkdir -p proyecto-gitignore/{src,build,dependencias}
cd proyecto-gitignore && git init -q
echo "# Proyecto de practica" > README.md
echo "programa de ejemplo" > src/programa.txt
echo "salida generada" > build/salida.txt
echo "registro" > registro.log
printf 'usuario=demo\nclave=NO-REAL-12345\n' > credenciales.txt
git add . && git commit -qm "Primer commit (se rastreo todo por descuido)"
echo "depuracion" > depuracion.log
```

### Comandos

```bash
printf '# Archivos generados\nbuild/\n*.log\n\n# Secretos (credenciales NO REAL)\ncredenciales.txt\n' > .gitignore
echo "clave=NO-REAL-67890" >> credenciales.txt
git status -s
git check-ignore -v credenciales.txt
git rm --cached credenciales.txt build/salida.txt registro.log
git add .gitignore
git commit -m "Dejar de rastrear archivos ignorados"
git status -s
ls
git ls-files
git check-ignore -v credenciales.txt depuracion.log
```

### Explicación

Aunque `credenciales.txt` está en el `.gitignore`, ya estaba rastreado, por eso Git lo ve
cambiar (`M` en la segunda columna) y `git check-ignore` no responde. `git rm --cached` lo saca del índice sin
borrarlo del disco. Desde el commit, `git check-ignore -v` responde con
`archivo:línea:patrón`.

**Paso 6 (respuesta esperada).** Si `credenciales.txt` ya estuviera publicado, dejar de
rastrearlo no borra sus versiones anteriores del historial remoto; habría que considerar
la credencial comprometida y reemplazarla (generar una nueva y revocar la anterior). Limpiar
el historial queda fuera de la clase.

### Resultado esperado

```console
$ git status -s
 M credenciales.txt
?? .gitignore
$ git check-ignore -v credenciales.txt
$ git status -s
$ ls
build  credenciales.txt  dependencias  depuracion.log  README.md  registro.log  src
$ git ls-files
.gitignore
README.md
src/programa.txt
$ git check-ignore -v credenciales.txt depuracion.log
.gitignore:6:credenciales.txt   credenciales.txt
.gitignore:3:*.log      depuracion.log
```

### Verificación

`git status` limpio, los archivos siguen en el disco, la lista de rastreados no incluye
los ignorados, y `check-ignore` identifica el archivo, la línea y el patrón.

---

## Desafío 01 — Conflictos y archivos ignorados en un mismo proyecto

### Preparación del ejercicio

```bash
cd diario-A
printf 'usuario=demo\nclave=NO-REAL-12345\n' > credenciales.txt
git add credenciales.txt
git commit -qm "Agregar credenciales de ejemplo (NO REAL) por descuido"
git push -q
cd ../diario-B && git pull -q
echo "registro" > registro.log
cd ../diario-A && echo "registro" > registro.log
```

### Comandos

```bash
# 1. Conflicto en pull
cd ../diario-B
sed -i '2s/.*/Entrada 1: instale Git y lo configure/' diario.txt
git commit -qam "Ampliar entrada 1 desde B"
git push -q
cd ../diario-A
sed -i '2s/.*/Entrada 1: instale Git en mi equipo/' diario.txt
git commit -qam "Reescribir entrada 1 desde A"
git pull
printf 'Diario de estudio\nEntrada 1: instale Git y lo configure en mi equipo\nEntrada 2: cree mi primera rama\nEntrada 3: aprendi a clonar\n' > diario.txt
git diff --check
git add diario.txt
git commit -m "Fusionar entrada 1 de A y B"
git push -q

# 2. Conflicto al revertir
echo "Entrada 4: Git borra el historial al hacer push" >> diario.txt
git commit -qam "Agregar entrada 4 con dato incorrecto"
sed -i '$s/.*/Entrada 4: Git borra el historial al hacer push (ampliado)/' diario.txt
git commit -qam "Ampliar entrada 4"
git push -q
H=$(git log --format=%h -1 --grep="entrada 4 con dato")
git revert --no-edit "$H"
git status -sb
printf 'Diario de estudio\nEntrada 1: instale Git y lo configure en mi equipo\nEntrada 2: cree mi primera rama\nEntrada 3: aprendi a clonar\n' > diario.txt
git add diario.txt
git revert --continue
git push -q

# 3. Ignorar archivos y dejar de rastrear credenciales.txt
printf '# Logs\n*.log\n\n# Secretos (credenciales NO REAL)\ncredenciales.txt\n' > .gitignore
git rm --cached credenciales.txt
git add .gitignore
git commit -m "Ignorar logs y dejar de rastrear credenciales de ejemplo"
git push -q
ls
git ls-files

# 4. Verificar en la copia B
cd ../diario-B
git pull -q
git status -s
cat .gitignore

# 5. Qué regla ignora un .log, y credenciales en el historial remoto
git check-ignore -v registro.log
git log --oneline -- credenciales.txt
```

### Explicación

**Paso 1.** Conflicto de contenido en el pull, resuelto combinando; `git diff --check` sin
salida antes de completar. **Paso 2.** El segundo commit modificó la misma línea que el
erróneo, así que `git revert` se detiene (`UU`); se deja el archivo sin la entrada y
`git revert --continue` crea la reversión. **Paso 3.** `git rm --cached` + `.gitignore`:
el archivo sigue en el disco. **Paso 4.** B recibe el `.gitignore` y su estado queda limpio
(el `registro.log` queda ignorado). **Paso 5.** `git check-ignore -v` muestra
`.gitignore:2:*.log`. `git log -- credenciales.txt` muestra que el archivo sigue en el
historial: dejar de rastrear no lo borra, y de ser real la credencial habría que
reemplazarla y revocar la anterior.

### Resultado esperado

```console
$ git pull
CONFLICT (content): Merge conflict in diario.txt
$ git revert --no-edit <id>
CONFLICT (content): Merge conflict in diario.txt
$ git status -sb
## master...origin/master
UU diario.txt
$ ls
configuracion.txt  credenciales.txt  diario.txt  registro.log
$ git ls-files
.gitignore
configuracion.txt
diario.txt
$ git status -s        # en B
$ git check-ignore -v registro.log
.gitignore:2:*.log      registro.log
$ git log --oneline -- credenciales.txt
<id> Ignorar logs y dejar de rastrear credenciales de ejemplo
<id> Agregar credenciales de ejemplo (NO REAL) por descuido
```

### Verificación

Se cumplen los casos de prueba del Desafío: conflicto en el pull, `git diff --check` sin
avisos, conflicto al revertir (`UU`), `credenciales.txt` fuera de `git ls-files` pero
presente en el disco, `.gitignore` versionado, B limpio y `check-ignore` identificando la
regla.
