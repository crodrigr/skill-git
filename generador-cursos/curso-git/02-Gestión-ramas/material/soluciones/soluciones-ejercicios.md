# Soluciones — Ejercicios (material docente)

No compartir este archivo con los estudiantes.

---

## Básico 01 — Gestión básica de ramas

```bash
git branch
git switch -c mi-rama
echo "un cambio" >> archivo.txt
git add archivo.txt
git commit -m "Agregar un cambio en mi-rama"

git switch master
cat archivo.txt   # todavia sin el cambio

git merge mi-rama -m "Fusionar mi-rama en master"
cat archivo.txt   # ahora si tiene el cambio

git branch -d mi-rama
git branch
```

**Verificación**: antes del merge, `archivo.txt` en `master` no tiene el cambio;
después, sí. `git branch -d mi-rama` se completa sin advertencias.

---

## Básico 02 — Fusión sin conflicto

```bash
git switch -c rama-a
echo "contenido" > archivo-nuevo.txt
git add archivo-nuevo.txt
git commit -m "Agregar archivo nuevo en rama-a"

git switch master
echo "otro cambio" >> archivo-existente.txt
git add archivo-existente.txt
git commit -m "Modificar archivo existente en master"

git log --oneline --all --graph
git merge rama-a -m "Fusionar rama-a en master"
ls
```

**Resultado esperado**: el merge se completa sin conflicto (archivos distintos);
`ls` muestra ambos archivos presentes después de la fusión.

---

## Intermedio 01 — Resolver un conflicto de fusión

```bash
git switch -c rama-conflicto
echo "version de rama-conflicto" > archivo.txt
git add archivo.txt
git commit -m "Cambiar la linea en rama-conflicto"

git switch master
echo "version de master" > archivo.txt
git add archivo.txt
git commit -m "Cambiar la linea en master"

git merge rama-conflicto
# CONFLICT (content): Merge conflict in archivo.txt

cat archivo.txt
# <<<<<<< HEAD
# version de master
# =======
# version de rama-conflicto
# >>>>>>> rama-conflicto

echo "version final combinada" > archivo.txt
git add archivo.txt
git commit -m "Fusionar rama-conflicto resolviendo conflicto en archivo.txt"
```

**Verificación**: antes de resolver, `git status` muestra "Unmerged paths"; el
archivo tiene los tres marcadores. Después de resolver y confirmar, `git status` ya
no muestra el conflicto y el archivo tiene el contenido final elegido, sin
marcadores.

---

## Intermedio 02 — Crear un repositorio remoto y conectarlo

```bash
# Remoto de practica (equivalente a crear un repositorio en GitHub):
git init --bare ../remoto-practica.git

git remote add origin ../remoto-practica.git
git remote -v
```

**Verificación**: `git remote -v` muestra `origin` con la ruta/URL correcta, tanto
para `fetch` como para `push`.

---

## Desafío 01 — Ciclo completo: rama, merge y remoto

```bash
# 1. Rama, trabajo y merge
git switch -c mejora
echo "una mejora" >> proyecto.txt
git add proyecto.txt
git commit -m "Agregar una mejora al proyecto"
git switch master
git merge mejora -m "Fusionar mejora en master"
git branch -d mejora

# 2. Crear remoto y conectar
git init --bare ../remoto-desafio.git
git remote add origin ../remoto-desafio.git

# 3. Publicar
git push -u origin master

# 4. Simular un cambio "desde otro lugar" (otro clon del mismo remoto)
cd ..
git clone remoto-desafio.git otra-copia
cd otra-copia
echo "cambio desde otro lugar" >> proyecto.txt
git add proyecto.txt
git commit -m "Agregar cambio desde otra copia"
git push origin master

# 5. Volver al original y traer el cambio
cd ../<carpeta-original>
git pull origin master
cat proyecto.txt

# 6. Historial final
git log --oneline --all --graph
```

**Resultado esperado**: `cat proyecto.txt` incluye tanto "una mejora" como "cambio
desde otro lugar"; `git log --oneline --all --graph` muestra el commit fusionado de
`mejora` y el commit traído del remoto, sin ningún commit perdido.

**Verificación**: aceptar cualquier proyecto propio del estudiante, siempre que
cumpla los 4 pasos (rama+merge, remoto+conexión, push, pull de un cambio ajeno) sin
mensajes de commit genéricos.
