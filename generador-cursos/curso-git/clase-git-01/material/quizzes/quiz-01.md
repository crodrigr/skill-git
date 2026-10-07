# Quiz 01 — Fundamentos Git

8 ítems. Sin respuestas (ver `material/soluciones/soluciones-quiz.md` para la clave,
material exclusivamente docente).

---

**1. [selección múltiple]**
¿Cuál de las siguientes opciones describe mejor qué es el control de versiones?

A. Un programa para editar código fuente.
B. Una práctica para registrar los cambios de un conjunto de archivos a lo largo del
   tiempo, permitiendo consultar el historial y volver a versiones anteriores.
C. Un servicio en la nube para guardar copias de seguridad automáticas.
D. Un lenguaje de programación usado para escribir scripts de automatización.

_RA: RA-1_

---

**2. [selección múltiple]**
¿Qué comando se usa para comprobar que Git está instalado correctamente?

A. `git install --check`
B. `git --version`
C. `git status`
D. `git verify`

_RA: RA-2_

---

**3. [selección múltiple]**
¿Qué comando configura el nombre de usuario que va a aparecer en los commits de todos
tus repositorios?

A. `git user --set-name "Tu Nombre"`
B. `git commit --name "Tu Nombre"`
C. `git config --global user.name "Tu Nombre"`
D. `git init --name "Tu Nombre"`

_RA: RA-3_

---

**4. [selección múltiple]**
¿Qué comando convierte una carpeta común en un repositorio de Git?

A. `git start`
B. `git new`
C. `git create`
D. `git init`

_RA: RA-4_

---

**5. [identificación de resultado]**
Partiendo de un repositorio recién inicializado (sin commits), creás un archivo nuevo
`notas.txt` pero todavía no ejecutaste ningún otro comando. ¿Qué va a mostrar
`git status` respecto de `notas.txt`?

_RA: RA-5_

---

**6. [identificación de resultado]**
En un repositorio donde se hicieron, en este orden, los commits "Agregar estructura
inicial" y luego "Agregar validacion de datos", ¿qué va a mostrar
`git log --oneline` (de arriba hacia abajo)?

_RA: RA-6_

---

**7. [corrección de errores]**
Un estudiante ejecuta esta secuencia y se sorprende de que `git log` no muestra ningún
commit nuevo:

```bash
echo "version 2" > notas.txt
git commit -m "Actualizar notas"
```

¿Qué paso le falta a esta secuencia, y por qué es necesario?

_RA: RA-5, RA-6_

---

**8. [problema breve]**
Escribí, en orden, la secuencia completa de comandos de Git necesaria para: crear un
repositorio nuevo en una carpeta vacía, crear un archivo `README.md` con un título
cualquiera, confirmarlo en un commit con un mensaje descriptivo, y por último mostrar
el historial del repositorio.

_RA: RA-4, RA-7_
