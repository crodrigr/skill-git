# Ejemplo 04 — Push rechazado: pull y reintento

## Estado inicial

Dos copias de `diario` sincronizadas con el remoto, en la rama `master`: tu copia
(`diario`) y la de otra persona (`otra-persona`). Se asume `pull.rebase false`. El
remoto es un repositorio `--bare` local en lugar de GitHub real.

## Comandos

```bash
# "Otra persona" publica primero:
#   echo "Entrada 4: otra persona" >> diario.txt
#   git commit -am "Agregar entrada 4 (otra persona)"
#   git push

# Vos, sin saberlo, confirmás un cambio en otro archivo:
echo "Resumen: clone y envie" > resumen.txt
git add resumen.txt
git commit -m "Agregar resumen"

# Intentás publicar: Git lo rechaza
git push

# Traés los cambios del remoto (puede abrirse el editor con el mensaje de fusión:
# guardá y cerrá)
git pull

# Comprobás el historial y reintentás
git log --oneline --graph
git push
```

## Explicación paso a paso

1. La otra persona publicó un commit. Ahora el remoto tiene un commit que tu copia no
   tiene; tu copia tiene uno (`resumen.txt`) que el remoto tampoco tiene: los historiales
   **divergen**.
2. `git push` es rechazado: `! [rejected] master -> master (fetch first)`. El mensaje
   explica que el remoto contiene trabajo que no tenés localmente. Git no te deja
   sobrescribirlo.
3. `git pull` trae el commit remoto y, como ambos lados tienen commits propios, crea un
   **commit de fusión** (`Merge made by the 'ort' strategy`; según tu versión de Git
   puede decir `'recursive'`). Como se modificaron archivos distintos, no hay conflicto.
   Si ambos hubieran tocado la misma línea, tendrías que resolver el conflicto como en
   la Clase 02.
4. `git log --oneline --graph` dibuja las dos líneas que se juntan en el commit de
   fusión.
5. Ahora el historial local contiene todo lo del remoto, y `git push` se acepta. La
   solución **nunca** es forzar el envío: eso borraría el commit de la otra persona.

## Resultado esperado

```console
$ git push
To https://github.com/tu-usuario/diario.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/tu-usuario/diario.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

$ git pull
From https://github.com/tu-usuario/diario
   e3d9824..072833b  master     -> origin/master
Merge made by the 'ort' strategy.
 diario.txt | 1 +
 1 file changed, 1 insertion(+)

$ git log --oneline --graph
*   293b92b Merge branch 'master' of https://github.com/tu-usuario/diario
|\
| * 072833b Agregar entrada 4 (otra persona)
* | 41c8532 Agregar resumen
|/
* e3d9824 Agregar entrada 3
* a17a2c4 Crear diario con dos entradas

$ git push
To https://github.com/tu-usuario/diario.git
   072833b..293b92b  master -> master
```

(Los identificadores serán distintos en tu repositorio.)
