# Ejemplo 02 — Merge sin conflicto

## Estado inicial

Un repositorio con un commit en `master` (`tareas.txt`). A partir de ahí, dos ramas
que avanzan en paralelo y modifican **archivos distintos**.

## Comandos

```bash
git switch -c agregar-prioridad
echo "Prioridad: alta" > prioridad.txt
git add prioridad.txt
git commit -m "Agregar archivo de prioridad"

git switch master
echo "- Comprar pan" >> tareas.txt
git add tareas.txt
git commit -m "Agregar primera tarea"

git log --oneline --all --graph

git merge agregar-prioridad -m "Fusionar agregar-prioridad en master"
ls
git log --oneline --all --graph
```

## Explicación paso a paso

1. Se crea la rama `agregar-prioridad` y, en ella, un commit que agrega
   `prioridad.txt`.
2. Se vuelve a `master` y se hace **otro** commit ahí, modificando `tareas.txt`. Esto
   hace que las dos ramas **diverjan**: cada una tiene un commit que la otra no
   tiene.
3. `git log --oneline --all --graph` muestra gráficamente esa divergencia: dos líneas
   que salen del mismo commit base.
4. Como ambas ramas modificaron archivos distintos, `git merge` combina todo sin
   pedir intervención manual, pero esta vez **sí** crea un commit de fusión real
   (no es fast-forward, porque `master` también avanzó): el mensaje "Merge made by
   the 'ort' strategy" lo confirma.
5. Después del merge, ambos archivos (`tareas.txt` y `prioridad.txt`) están presentes,
   y el historial muestra el commit de fusión uniendo las dos líneas.

## Resultado esperado

```console
$ git log --oneline --all --graph
* 0bb96b3 Agregar archivo de prioridad
| * 60645b6 Agregar primera tarea
|/
* 9918f9d Agregar lista de tareas

$ git merge agregar-prioridad -m "Fusionar agregar-prioridad en master"
Merge made by the 'ort' strategy.
 prioridad.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 prioridad.txt

$ ls
prioridad.txt  tareas.txt

$ git log --oneline --all --graph
*   dadc067 Fusionar agregar-prioridad en master
|\
| * 0bb96b3 Agregar archivo de prioridad
* | 60645b6 Agregar primera tarea
|/
* 9918f9d Agregar lista de tareas
```

(Los identificadores de commit van a ser distintos en tu propio repositorio.)
