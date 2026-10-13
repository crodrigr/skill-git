# 🔑 Soluciones — Quiz 01 (material docente)

No compartir este archivo con los estudiantes.

---

**1.** Respuesta correcta: **C**.
Justificación: hay conflicto de contenido cuando ambos lados cambiaron las mismas líneas.
Archivos distintos (A), archivos nuevos (B) o una rama con más commits (D) se fusionan
solos.

**2.** Respuesta correcta: **B** (`git status`).
Justificación: separa `Unmerged paths` (requieren decisión) de `Changes to be committed`
(ya fusionados). Las otras opciones no informan sobre el estado de la fusión.

**3.** Respuesta correcta: **C**.
Justificación: `git merge --abort` regresa al estado previo a la fusión; los commits
locales se conservan. No elimina commits (A), no confirma (B) ni borra ramas (D).

**4.** Respuesta correcta: **B**.
Justificación: `.gitignore` lista patrones de archivos que Git no rastrea. No borra
archivos (A), no controla quién clona (C) ni resuelve conflictos (D).

**5.** Respuesta esperada: la versión **propia** es la de arriba (entre `<<<<<<< HEAD` y
`=======`): "escribir un resumen"; la **entrante** es la de abajo (entre `=======` y
`>>>>>>> mejoras`): "escribir un informe". Una combinación válida:

```text
Tarea 1: leer
Tarea 2: escribir un resumen y un informe
Tarea 3: practicar
```

Justificación: tras decidir hay que eliminar las tres líneas de marcadores.

**6.** Respuesta esperada: `.gitignore` es el archivo de reglas que ignora el archivo
consultado; `3` es el número de línea; `*.log` es el patrón que coincide; y
`depuracion.log` es el archivo consultado. En conjunto: ese archivo se ignora por la regla
de la línea 3.

**7.** Respuesta esperada: agregar un archivo al `.gitignore` no deja de rastrear un
archivo que **ya estaba rastreado**; el `.gitignore` solo evita rastrear archivos nuevos.
Secuencia correcta:

```bash
git rm --cached credenciales.txt
git add .gitignore
git commit -m "Dejar de rastrear credenciales"
git push
```

Justificación: `--cached` conserva el archivo en el disco. Si la credencial fuera real,
le falta **reemplazarla** (generar una nueva y revocar la anterior): las versiones
anteriores siguen en el historial publicado.

**8.** Respuesta esperada (admite variantes equivalentes):

```bash
git status                                   # (a) archivo en "Unmerged paths"
# editar diario.txt y eliminar los marcadores
git diff --check                             # (b) sin avisos = sin marcadores
git add diario.txt                           # marcar como resuelto
git commit -m "Fusionar cambios del remoto"  # (c) completar la fusión
git push                                     # (d) publicar
```

Justificación: `git diff --check` (o un `grep` de los marcadores) evita confirmar un archivo
roto.
