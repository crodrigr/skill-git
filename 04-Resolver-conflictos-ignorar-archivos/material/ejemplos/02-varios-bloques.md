# 📘 Ejemplo 02 — Varios bloques de conflicto en un archivo

## 🧱 Estado inicial

Un archivo `lista.txt` de 14 líneas (título y trece tareas), confirmado en `master`. La
rama `cambios-lista` y `master` modificaron, cada una por su cuenta, las líneas de la
tarea 1, la tarea 7 y la tarea 13. Estás parado en `master`.

## 💻 Comandos

```bash
# Fusionar: conflicto con tres bloques en el mismo archivo
git merge cambios-lista
cat lista.txt

# Ubicar los marcadores
grep -nE '^(<<<<<<<|=======|>>>>>>>)' lista.txt

# Si confirmaras con marcadores sin resolver, Git te lo advierte:
git diff --check

# Resolver los tres bloques (equivale a editar a mano):
#   bloque 1 -> lado propio, bloque 2 -> lado entrante, bloque 3 -> combinar
{ echo "Lista de tareas"; echo "Tarea 1: leer en voz alta"
  for i in 2 3 4 5 6; do echo "Tarea $i: pendiente"; done
  echo "Tarea 7: repasar los apuntes"
  for i in 8 9 10 11 12; do echo "Tarea $i: pendiente"; done
  echo "Tarea 13: caminar un rato al aire libre"; } > lista.txt

# Comprobar que no quedan marcadores
grep -nE '^(<<<<<<<|=======|>>>>>>>)' lista.txt
git diff --check

# Marcar como resuelto y completar la fusión
git add lista.txt
git commit -m "Fusionar cambios-lista"
```

## 🔍 Explicación paso a paso

1. Como las tres modificaciones están separadas por varias líneas sin tocar, Git las trata
   como **tres bloques independientes** dentro del mismo archivo: cada uno tiene su propio
   `<<<<<<<`, `=======` y `>>>>>>>`.
2. El `grep` lista las líneas de los marcadores (nueve en total: tres por bloque). Es útil
   en archivos largos para no dejar ninguno.
3. `git diff --check` detecta los marcadores que quedaron y responde `leftover conflict
   marker`. Si lo ejecutás **antes** de confirmar, evitás publicar un archivo roto.
4. Cada bloque se decide por separado:
   - Bloque 1 (tarea 1): **lado propio**, "leer en voz alta".
   - Bloque 2 (tarea 7): **lado entrante**, "repasar los apuntes".
   - Bloque 3 (tarea 13): **combinar**, "caminar un rato al aire libre", que recoge las
     dos ideas.
5. Tras editar, `grep` ya no devuelve líneas (código de salida 1) y `git diff --check`
   termina sin avisos: el archivo está limpio.
6. `git add lista.txt` lo marca como resuelto y `git commit` completa la fusión.

## ✅ Resultado esperado

```console
$ git merge cambios-lista
Auto-merging lista.txt
CONFLICT (content): Merge conflict in lista.txt
Automatic merge failed; fix conflicts and then commit the result.

$ cat lista.txt
Lista de tareas
<<<<<<< HEAD
Tarea 1: leer en voz alta
=======
Tarea 1: leer el capitulo
>>>>>>> cambios-lista
Tarea 2: pendiente
Tarea 3: pendiente
Tarea 4: pendiente
Tarea 5: pendiente
Tarea 6: pendiente
<<<<<<< HEAD
Tarea 7: repasar con tarjetas
=======
Tarea 7: repasar los apuntes
>>>>>>> cambios-lista
Tarea 8: pendiente
Tarea 9: pendiente
Tarea 10: pendiente
Tarea 11: pendiente
Tarea 12: pendiente
<<<<<<< HEAD
Tarea 13: caminar un rato
=======
Tarea 13: caminar al aire libre
>>>>>>> cambios-lista

$ grep -nE '^(<<<<<<<|=======|>>>>>>>)' lista.txt
2:<<<<<<< HEAD
4:=======
6:>>>>>>> cambios-lista
12:<<<<<<< HEAD
14:=======
16:>>>>>>> cambios-lista
22:<<<<<<< HEAD
24:=======
26:>>>>>>> cambios-lista

$ git diff --check
lista.txt:2: leftover conflict marker
lista.txt:4: leftover conflict marker
lista.txt:6: leftover conflict marker
lista.txt:12: leftover conflict marker
lista.txt:14: leftover conflict marker
lista.txt:16: leftover conflict marker
lista.txt:22: leftover conflict marker
lista.txt:24: leftover conflict marker
lista.txt:26: leftover conflict marker

$ grep -nE '^(<<<<<<<|=======|>>>>>>>)' lista.txt
$ git diff --check
$ git add lista.txt
$ git commit -m "Fusionar cambios-lista"
[master 54455cc] Fusionar cambios-lista
```

> 💡 Los identificadores serán distintos en tu repositorio. Los comandos `grep` y
> `git diff --check` sin salida indican que no quedan marcadores.
