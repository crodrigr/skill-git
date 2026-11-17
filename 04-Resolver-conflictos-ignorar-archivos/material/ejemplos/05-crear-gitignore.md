# 📘 Ejemplo 05 — Crear un .gitignore con distintos patrones

## 🧱 Estado inicial

Una carpeta de práctica `proyecto-gitignore`, con un repositorio recién iniciado (`git
init`) y estos archivos, ninguno confirmado todavía:

```text
README.md
auditoria.log          # un log que SÍ queremos versionar
borrador.txt           # borrador de la raíz
build/salida.txt       # archivo generado
credenciales.txt       # credenciales de ejemplo (NO REAL)
dependencias/libreria.txt
depuracion.log
registro.log
src/programa.txt
src/docs/borrador.txt  # otro borrador, en una subcarpeta
```

El contenido de `credenciales.txt` es ficticio (`clave=NO-REAL-12345`): en este curso
nunca se usan credenciales reales.

## 💻 Comandos

```bash
# Antes: todo aparece como no rastreado
git status -s

# Crear el archivo de exclusiones
cat > .gitignore <<'G'
# Archivos generados
build/
*.log
!auditoria.log

# Dependencias
dependencias/

# Secretos (en este curso, credenciales NO REAL)
credenciales.txt

# Solo el borrador de la raiz
/borrador.txt
G

# Después: solo queda lo que sí debe versionarse
git status -s

# Ver también lo que Git ignora
git status --ignored -s

# Confirmar y listar lo que quedó rastreado
git add .
git commit -m "Crear proyecto con .gitignore"
git ls-files
```

## 🔍 Explicación paso a paso

1. `git status -s` antes de crear el archivo muestra todo como no rastreado (`??`),
   incluidas la carpeta generada, las dependencias y las credenciales: es lo que **no**
   querés confirmar por descuido.
2. En el `.gitignore` cada línea es un patrón:
   - `build/` ignora la carpeta completa.
   - `*.log` ignora todos los archivos terminados en `.log` (por comodín).
   - `!auditoria.log` es una **excepción**: aunque `*.log` lo ignoraría, ese archivo sí se
     rastrea. La excepción va después del patrón que anula.
   - `dependencias/` ignora la carpeta de dependencias.
   - `credenciales.txt` ignora ese nombre exacto.
   - `/borrador.txt` ignora el borrador **solo en la raíz**; el de `src/docs/` sigue
     rastreable.
   - Las líneas con `#` son comentarios y las vacías se ignoran.
3. `git status -s` ahora muestra solo `.gitignore`, `README.md`, `auditoria.log` y
   `src/`. El propio `.gitignore` aparece porque también se versiona.
4. `git status --ignored -s` marca con `!!` lo que Git ignora. Sirve para comprobar que
   los patrones hacen lo que esperás.
5. `git ls-files` lista lo que quedó rastreado tras el commit: los archivos ignorados no
   están, y `src/docs/borrador.txt` sí, porque el patrón de la raíz no le alcanza.

## ✅ Resultado esperado

```console
$ git status -s
?? README.md
?? auditoria.log
?? borrador.txt
?? build/
?? credenciales.txt
?? dependencias/
?? depuracion.log
?? registro.log
?? src/

$ git status -s
?? .gitignore
?? README.md
?? auditoria.log
?? src/

$ git status --ignored -s
?? .gitignore
?? README.md
?? auditoria.log
?? src/
!! borrador.txt
!! build/
!! credenciales.txt
!! dependencias/
!! depuracion.log
!! registro.log

$ git ls-files
.gitignore
README.md
auditoria.log
src/docs/borrador.txt
src/programa.txt
```

> 💡 El orden de las líneas de `git status` puede variar levemente entre versiones de Git.
