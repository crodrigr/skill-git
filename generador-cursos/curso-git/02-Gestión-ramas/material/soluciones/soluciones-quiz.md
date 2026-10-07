# Soluciones — Quiz 01 (material docente)

No compartir este archivo con los estudiantes.

---

**1.** Respuesta correcta: **B**.
Justificación: una rama es una línea de desarrollo paralela dentro del mismo
repositorio; no es una copia separada (eso describiría mejor a un clon), ni un
archivo de configuración, ni el nombre de un remoto.

**2.** Respuesta correcta: **C** (`git switch -c arreglo`).
Justificación: `-c` le dice a `git switch` que cree la rama si no existe y cambie a
ella en el mismo paso; las otras opciones no son comandos reales de Git.

**3.** Respuesta correcta: **A** (`git branch -d nombre-rama`).
Justificación: `-d` es la eliminación segura (rechaza si hay trabajo sin fusionar);
las otras opciones no son la sintaxis real de Git.

**4.** Respuesta correcta: **C** (`git remote add origin <url>`).
Justificación: es el comando que asocia una URL remota con un nombre dentro del
repositorio local.

**5.** Respuesta esperada: "version A" (entre `<<<<<<< HEAD` y `=======`) es el
contenido de la rama en la que estabas parado cuando hiciste el merge; "version B"
(entre `=======` y `>>>>>>> otra-rama`) es el contenido de la rama que se intentó
fusionar.

**6.** Respuesta esperada: el remoto tiene al menos un commit que el repositorio
local no tiene (alguien más publicó cambios, o vos mismo desde otra computadora), y
Git rechaza el `push` para no sobrescribirlo. Antes de reintentar, hay que ejecutar
`git pull` (traer e integrar esos cambios) y recién después volver a hacer `git
push`.

**7.** Respuesta esperada: Git rechazó el `-d` porque `mi-rama` tiene commits que
todavía no están fusionados a ninguna otra rama; eliminarla así perdería ese
trabajo. Las dos opciones: (a) fusionar primero la rama (con `git merge`) y luego
eliminarla con `-d`, que es lo seguro; o (b) si de verdad se quiere descartar ese
trabajo a propósito, forzar la eliminación con `git branch -D`, sabiendo que eso
borra los commits sin fusionar.

**8.** Respuesta esperada (acepta variaciones de nombres y mensajes, siempre que el
orden de comandos sea correcto):

```bash
git switch -c mi-rama
git commit -m "..."     # tras modificar algo
git switch master
git merge mi-rama -m "..."
git remote add origin <url-o-ruta>
git push -u origin master
```
