# Quiz 01 — Gestión de Ramas

8 ítems. Sin respuestas (ver `material/soluciones/soluciones-quiz.md` para la clave,
material exclusivamente docente).

---

**1. [selección múltiple]**
¿Cuál de las siguientes opciones describe mejor qué es una rama en Git?

A. Una copia completa y separada del repositorio, en otra carpeta.
B. Una línea de desarrollo paralela dentro del mismo repositorio, que permite
   aislar cambios sin afectar la rama principal hasta fusionarlos.
C. Un archivo que guarda la configuración del repositorio.
D. El nombre que recibe el repositorio remoto principal.

_RA: RA-1_

---

**2. [selección múltiple]**
¿Qué comando crea una rama nueva llamada `arreglo` y cambia a ella en un solo paso?

A. `git branch arreglo --switch`
B. `git merge arreglo`
C. `git switch -c arreglo`
D. `git remote add arreglo`

_RA: RA-3_

---

**3. [selección múltiple]**
¿Qué comando elimina de forma segura una rama que ya fue fusionada?

A. `git branch -d nombre-rama`
B. `git branch --remove nombre-rama`
C. `git merge --delete nombre-rama`
D. `git switch -d nombre-rama`

_RA: RA-3_

---

**4. [selección múltiple]**
¿Qué comando conecta un repositorio local existente con un repositorio remoto?

A. `git push --connect <url>`
B. `git clone --local <url>`
C. `git remote add origin <url>`
D. `git init --remote <url>`

_RA: RA-2_

---

**5. [identificación de resultado]**
Después de un `git merge` que termina en conflicto, abrís el archivo afectado y ves
esto:

```text
<<<<<<< HEAD
version A
=======
version B
>>>>>>> otra-rama
```

¿Qué representa "version A" y qué representa "version B"?

_RA: RA-3_

---

**6. [identificación de resultado]**
Intentás `git push origin master` y Git responde con un mensaje que incluye
"Updates were rejected because the remote contains work that you do not have
locally". ¿Qué significa ese mensaje, y qué comando deberías ejecutar antes de
volver a intentar el `push`?

_RA: RA-4_

---

**7. [corrección de errores]**
Un estudiante termina de trabajar en una rama sin fusionarla todavía y ejecuta:

```bash
git branch -d mi-rama
```

Git responde con un error y no elimina la rama. ¿Por qué pasó esto, y qué dos
opciones tiene el estudiante para seguir adelante (una seguridad, no forzar)?

_RA: RA-3_

---

**8. [problema breve]**
Escribí, en orden, la secuencia completa de comandos de Git necesaria para: crear una
rama nueva, hacer un commit en ella, volver a la rama principal, fusionar la rama,
crear un repositorio remoto nuevo y conectarlo, y por último publicar el historial en
ese remoto.

_RA: RA-1, RA-2, RA-3, RA-4_
