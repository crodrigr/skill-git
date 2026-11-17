# 📘 Ejemplo 06 — Dejar de rastrear un archivo y averiguar por qué se ignora

## 🧱 Estado inicial

Un repositorio de práctica `proyecto-gitignore` donde, por descuido, ya se confirmó todo
en el primer commit: `README.md`, `src/programa.txt`, `build/salida.txt`, `registro.log`
y `credenciales.txt` (contenido ficticio y **NO REAL**: `clave=NO-REAL-12345`). Recién
después se crea un `.gitignore`.

## 💻 Comandos

```bash
# Crear el .gitignore (tarde: los archivos ya estaban rastreados)
printf '# Archivos generados\nbuild/\n*.log\n\n# Secretos (credenciales NO REAL)\ncredenciales.txt\n' > .gitignore
git status -s

# Modificar un archivo "ignorado": Git igual lo ve cambiar
echo "clave=NO-REAL-67890" >> credenciales.txt
git status -s
git check-ignore -v credenciales.txt
git ls-files

# Dejar de rastrear, sin borrarlos del disco
git rm --cached credenciales.txt build/salida.txt registro.log
git status -s
ls

# Confirmar el cambio y el .gitignore
git add .gitignore
git commit -m "Dejar de rastrear archivos ignorados"
git status -s
git ls-files

# Ahora sí: averiguar qué regla ignora cada archivo
git check-ignore -v credenciales.txt build/salida.txt registro.log

# Forzar una excepción puntual
git add credenciales.txt
git add -f registro.log
git status -s
git reset -q registro.log

# Ignorar algo solo en tu máquina (no se comparte)
echo "notas-personales.txt" >> .git/info/exclude
echo "mis notas" > notas-personales.txt
git status -s
git check-ignore -v notas-personales.txt
```

## 🔍 Explicación paso a paso

1. Aunque `credenciales.txt` está en el `.gitignore`, ya estaba **rastreado**. Al
   modificarlo, `git status -s` muestra `M` en la segunda columna para `credenciales.txt`: el `.gitignore` solo
   evita rastrear archivos nuevos.
2. `git check-ignore -v credenciales.txt` no imprime nada (código de salida 1): para Git
   ese archivo **no está ignorado** porque sigue rastreado. `git ls-files` lo confirma.
3. `git rm --cached` quita los archivos del índice **sin borrarlos del disco**. En
   `git status -s` aparecen con `D` en la primera columna (eliminación preparada) y `ls` demuestra que siguen
   en la carpeta. Sin `--cached` también se borrarían del disco.
4. Se confirma el cambio junto con el `.gitignore`. Desde ese commit, el estado queda
   limpio y `git ls-files` ya no lista los archivos ignorados.
5. Ahora `git check-ignore -v` sí responde, con el formato `archivo:línea:patrón` seguido
   del archivo consultado. Es la forma de averiguar qué regla ignora algo (por ejemplo,
   si un patrón demasiado amplio oculta un archivo que sí querías versionar).
6. `git add credenciales.txt` falla con el aviso de que el archivo está ignorado, y
   `git add -f registro.log` lo fuerza. Es una excepción puntual; si necesitás hacerlo
   seguido, mejor corregí el patrón con una negación (`!`).
7. `.git/info/exclude` funciona como un `.gitignore` **personal**: no se versiona ni se
   comparte. Es el lugar para tus archivos propios.

**Si `credenciales.txt` ya se hubiera publicado en el remoto**, dejar de rastrearlo no
borra sus versiones anteriores del historial: se considerarían comprometidas y habría que
reemplazarlas. Eso se explica en la clase, pero no se practica.

## ✅ Resultado esperado

```console
$ git status -s
?? .gitignore

$ echo "clave=NO-REAL-67890" >> credenciales.txt
$ git status -s
 M credenciales.txt
?? .gitignore

$ git check-ignore -v credenciales.txt
$ git ls-files
README.md
build/salida.txt
credenciales.txt
registro.log
src/programa.txt

$ git rm --cached credenciales.txt build/salida.txt registro.log
rm 'build/salida.txt'
rm 'credenciales.txt'
rm 'registro.log'

$ git status -s
D  build/salida.txt
D  credenciales.txt
D  registro.log
?? .gitignore

$ ls
build  credenciales.txt  README.md  registro.log  src

$ git commit -m "Dejar de rastrear archivos ignorados"
[master 8617871] Dejar de rastrear archivos ignorados
 4 files changed, 6 insertions(+), 4 deletions(-)
 create mode 100644 .gitignore
 delete mode 100644 build/salida.txt
 delete mode 100644 credenciales.txt

$ git status -s
$ git ls-files
.gitignore
README.md
src/programa.txt

$ git check-ignore -v credenciales.txt build/salida.txt registro.log
.gitignore:6:credenciales.txt   credenciales.txt
.gitignore:2:build/     build/salida.txt
.gitignore:3:*.log      registro.log

$ git add credenciales.txt
The following paths are ignored by one of your .gitignore files:
credenciales.txt
hint: Use -f if you really want to add them.

$ git add -f registro.log
$ git status -s
A  registro.log

$ git check-ignore -v notas-personales.txt
.git/info/exclude:7:notas-personales.txt        notas-personales.txt
```

> 💡 En `git commit` se omite la línea `delete mode` de `registro.log` por brevedad. El número
> de línea de `.git/info/exclude` depende del contenido del archivo en tu repositorio. En
> las salidas reales de `check-ignore`, una tabulación separa la regla del archivo.
