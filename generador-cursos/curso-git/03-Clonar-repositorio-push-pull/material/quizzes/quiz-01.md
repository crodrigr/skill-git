# Quiz 01 — Clonar un repositorio, hacer push y pull

8 ítems. Sin respuestas (ver `material/soluciones/soluciones-quiz.md` para la clave,
material exclusivamente docente).

---

**1. [selección múltiple]**
¿Qué obtenés al clonar un repositorio remoto con `git clone`?

A. Solo los archivos de la última versión, sin historial.
B. Los archivos, todo el historial de commits y el remoto de origen ya configurado
   como `origin`.
C. Una carpeta vacía que luego debés conectar con `git remote add`.
D. Únicamente la rama principal, sin acceso a las demás ramas remotas.

_RA: RA-1_

---

**2. [selección múltiple]**
¿Cuál de las siguientes afirmaciones describe correctamente la diferencia entre
`git pull` y `git push`?

A. `pull` envía tus commits al remoto y `push` descarga los del remoto.
B. `pull` trae los commits nuevos del remoto a tu repositorio y `push` publica en el
   remoto los commits que este aún no tiene.
C. Ambos descargan cambios; `push` solo lo hace con ramas nuevas.
D. `pull` borra los commits locales y `push` borra los remotos.

_RA: RA-2, RA-3_

---

**3. [selección múltiple]**
Un `git push` es rechazado con `! [rejected] master -> master (fetch first)`. ¿Cuál es
la causa más probable?

A. Tu token tiene permisos de lectura, pero no de escritura.
B. El repositorio remoto no existe.
C. El remoto tiene commits que tu repositorio local no tiene.
D. Tenés archivos sin confirmar en tu carpeta de trabajo.

_RA: RA-3_

---

**4. [selección múltiple]**
Publicaste por error un commit con un dato incorrecto. ¿Qué opción corrige el error sin
reescribir el historial compartido?

A. `git revert <commit>` y luego `git push`.
B. Borrar el commit del historial local y forzar el envío.
C. Clonar el repositorio otra vez y descartar el anterior.
D. `git pull` repetidas veces hasta que el error desaparezca.

_RA: RA-4_

---

**5. [identificación de resultado]**
Acabas de clonar `https://github.com/docente-git/proyecto-ejemplo.git` y ejecutás
`git remote -v`. Escribí qué líneas esperás ver y explicá por qué no ejecutaste antes
ningún `git remote add`.

_RA: RA-1_

---

**6. [identificación de resultado]**
Observá este mensaje de Git al hacer `git push`:

```console
 ! [rejected]        master -> master (fetch first)
hint: Updates were rejected because the remote contains work that you do
hint: not have locally.
```

Explicá con tus palabras qué ocurrió y cuál es el siguiente comando que debés ejecutar.

_RA: RA-3, RA-2_

---

**7. [corrección de errores]**
Una compañera quiere publicar su commit, pero el envío es rechazado. Ella ejecuta esta
secuencia para "arreglarlo":

```bash
git push
git push --force
```

Identificá qué está mal en su enfoque, qué podría perder el equipo y cuál es la
secuencia correcta.

_RA: RA-3, RA-4_

---

**8. [problema breve]**
Un commit con un error ya está publicado en `master`. Escribí la secuencia completa de
comandos para (a) localizar el commit, (b) revertirlo, (c) publicar la corrección y
(d) hacer que otra persona que tiene su propia copia la reciba.

_RA: RA-4, RA-2_
