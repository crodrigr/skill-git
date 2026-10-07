# Soluciones — Ejercicios (material docente)

No compartir este archivo con los estudiantes.

---

## Básico 01 — Instalar y configurar Git

```bash
git --version
# si no esta instalado: sudo apt install git (Linux) / brew install git (macOS) /
# instalador de Git for Windows

git config --global user.name "Tu Nombre"
git config --global user.email "tu-correo@ejemplo.com"
git config --list
```

**Explicación**: `git --version` confirma la instalación; si falla, se instala según
el sistema operativo. `git config --global` guarda la identidad una sola vez para
todos los repositorios de esa computadora.

**Verificación**: `git config --list` debe mostrar `user.name` y `user.email` con los
valores configurados.

---

## Básico 02 — Explicar el control de versiones

**Respuesta de referencia** (la respuesta del estudiante puede variar en redacción y
ejemplo, siempre que cubra estas tres ideas):

1. El control de versiones registra los cambios de un conjunto de archivos a lo largo
   del tiempo.
2. Resuelve, por ejemplo, el problema de no saber cuál de varias copias de un archivo
   es la "buena", o de perder un cambio que funcionaba antes.
3. Sin control de versiones, un proyecto de código puede perder cambios, mezclar
   versiones incompatibles entre varias personas, o no tener forma de volver a un
   estado anterior que funcionaba.

**Verificación**: aceptar cualquier ejemplo propio coherente con estas tres ideas; no
exigir la misma redacción que el material.

---

## Intermedio 01 — Flujo local completo

```bash
mkdir mi-ejercicio && cd mi-ejercicio
git init

echo "Primera version" > notas.txt
git add notas.txt
git commit -m "Agregar notas iniciales"

echo "Primera version" >> notas.txt
echo "Segunda linea agregada" >> notas.txt
git add notas.txt
git commit -m "Agregar segunda linea a las notas"

echo "contenido" > otro-archivo.txt
git add otro-archivo.txt
git commit -m "Agregar otro archivo al proyecto"

git log --oneline
git status
```

**Resultado esperado**: `git log --oneline` muestra 3 commits con mensajes distintos;
`git status` no muestra cambios pendientes.

---

## Intermedio 02 — Navegación del historial

```bash
git log
git show <identificador-del-commit-mas-antiguo>
# o, equivalente, usando una referencia relativa:
git show HEAD~2
```

**Explicación**: `git log` lista los 3 commits del ejercicio anterior; `git show`
sobre el commit más antiguo (o `HEAD~2`, "dos commits antes del más reciente") muestra
el archivo `notas.txt` con el contenido "Primera version" agregado.

**Verificación**: la descripción escrita por el estudiante debe coincidir con el
archivo y el contenido que realmente aparece en la salida de `git show`.

---

## Desafío 01 — Un repositorio de principio a fin

```bash
mkdir mi-desafio && cd mi-desafio
git init

echo "Lista de tareas" > tareas.txt
git add tareas.txt
git commit -m "Agregar estructura inicial de la lista de tareas"

echo "- Comprar pan" >> tareas.txt
git add tareas.txt
git commit -m "Agregar primera tarea"

cat > README.md <<'EOF'
# Lista de tareas

Un archivo de texto simple para anotar pendientes del dia a dia.
EOF
git add README.md
git commit -m "Agregar README con descripcion del proyecto"

echo "- Pagar servicios" >> tareas.txt
git add tareas.txt
git commit -m "Agregar segunda tarea"

git log --oneline
git show HEAD~1 --stat   # commit intermedio: el del README
```

**Resultado esperado**: 4 commits en `git log --oneline`, cada uno con mensaje
distinto; `README.md` presente y confirmado; `git show HEAD~1` (o el identificador
correspondiente) muestra exactamente el commit del README como contenido intermedio.

**Verificación**: aceptar cualquier proyecto propio del estudiante, siempre que cumpla
el mínimo de 4 commits distintos, el README confirmado, y una descripción correcta del
commit intermedio elegido.
