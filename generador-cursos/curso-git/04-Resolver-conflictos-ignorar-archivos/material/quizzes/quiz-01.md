# Quiz 01 — Resolver conflictos e ignorar archivos

8 ítems. Sin respuestas (ver `material/soluciones/soluciones-quiz.md` para la clave,
material exclusivamente docente).

---

**1. [selección múltiple]**
¿En cuál de estas situaciones Git detiene una fusión y marca un conflicto?

A. Cuando dos ramas modificaron archivos distintos.
B. Cuando una rama agregó un archivo nuevo y la otra no lo tocó.
C. Cuando las dos ramas modificaron las mismas líneas de un archivo.
D. Cuando una de las ramas tiene más commits que la otra.

_RA: RA-1_

---

**2. [selección múltiple]**
Una fusión se detuvo con conflictos. ¿Qué comando te muestra qué archivos requieren tu
decisión y cuáles ya se fusionaron solos?

A. `git log --oneline`
B. `git status`
C. `git branch -a`
D. `git remote -v`

_RA: RA-1_

---

**3. [selección múltiple]**
Estás en medio de una fusión con conflictos y decidís no continuar. ¿Qué hace
`git merge --abort`?

A. Elimina tus commits locales para quedar igual que el remoto.
B. Confirma la fusión dejando los marcadores en los archivos.
C. Vuelve al estado previo a la fusión, sin perder tus commits.
D. Borra la rama que intentabas fusionar.

_RA: RA-1_

---

**4. [selección múltiple]**
¿Para qué sirve el archivo `.gitignore`?

A. Para borrar del disco los archivos que se ignoran.
B. Para indicarle a Git qué archivos no debe rastrear.
C. Para impedir que otras personas clonen el repositorio.
D. Para resolver conflictos de fusión de forma automática.

_RA: RA-2_

---

**5. [identificación de resultado]**
Observá este archivo después de una fusión con conflicto:

```text
Tarea 1: leer
<<<<<<< HEAD
Tarea 2: escribir un resumen
=======
Tarea 2: escribir un informe
>>>>>>> mejoras
Tarea 3: practicar
```

Indicá cuál es la versión propia y cuál la entrante, y escribí cómo quedaría el archivo si
decidís **combinar** ambas.

_RA: RA-1_

---

**6. [identificación de resultado]**
El proyecto tiene un `.gitignore` con la línea `*.log`, y ejecutás:

```console
$ git check-ignore -v depuracion.log
.gitignore:3:*.log      depuracion.log
```

Explicá con tus palabras qué te dice cada parte de la respuesta.

_RA: RA-2_

---

**7. [corrección de errores]**
Un compañero confirmó por descuido un archivo `credenciales.txt` (con datos de prueba) y
ya lo publicó. Después lo agregó al `.gitignore` y ejecutó `git status`: el archivo sigue
apareciendo cuando lo modifica. Él piensa que Git "ignora el `.gitignore`".

Identificá qué está mal en su enfoque y escribí la secuencia correcta de comandos para
dejar de rastrear el archivo sin borrarlo del disco. Explicá además qué le falta resolver
si la credencial fuera real.

_RA: RA-2_

---

**8. [problema breve]**
Hacés `git pull` y termina en conflicto en `diario.txt`. Escribí la secuencia completa de
comandos para: (a) identificar el archivo en conflicto, (b) comprobar que no quedan
marcadores una vez resuelto, (c) completar la fusión y (d) publicar el resultado.

_RA: RA-1_
