# Ejemplo 05 — Revertir un commit y propagarlo al remoto

## Estado inicial

Dos copias de `diario` sincronizadas con el remoto, en `master`: tu copia (`diario`) y
la de otra persona (`otra-persona`). Acabas de publicar un commit con un dato
incorrecto ("Entrada 5 con dato incorrecto") que ya está en el remoto. El remoto es un
repositorio `--bare` local en lugar de GitHub real.

## Comandos

```bash
# Localizar el commit erróneo
git log --oneline

# Revertirlo (crea un commit nuevo que lo deshace)
git revert --no-edit HEAD
git log --oneline
cat diario.txt

# Propagar la corrección al remoto
git push

# La otra persona la recibe:
cd ../otra-persona
git pull
git log --oneline
```

## Explicación paso a paso

1. `git log --oneline` permite identificar el commit erróneo. Aquí es el último
   (`HEAD`); si no lo fuera, copiarías su identificador.
2. `git revert --no-edit HEAD` crea un **commit nuevo** (`Revert "..."`) cuyo contenido
   deshace el cambio. `--no-edit` acepta el mensaje que Git propone en lugar de abrir
   el editor.
3. `git log --oneline` muestra el commit original **y** el de reversión: el historial no
   se reescribió, solo creció. `cat diario.txt` confirma que la línea incorrecta
   desapareció del archivo.
4. Hasta aquí la corrección es solo local. `git push` la publica: el remoto queda
   corregido y conserva ambos commits.
5. En la copia de la otra persona, `git pull` trae el commit de reversión con un avance
   directo (*Fast-forward*): recibe la corrección sin conflictos, porque tu push no
   modificó commits ya publicados.

## Resultado esperado

```console
$ git log --oneline
c563374 Agregar entrada 5 con dato incorrecto
293b92b Merge branch 'master' of https://github.com/tu-usuario/diario
41c8532 Agregar resumen

$ git revert --no-edit HEAD
[master 7940518] Revert "Agregar entrada 5 con dato incorrecto"
 1 file changed, 1 deletion(-)

$ git log --oneline
7940518 Revert "Agregar entrada 5 con dato incorrecto"
c563374 Agregar entrada 5 con dato incorrecto
293b92b Merge branch 'master' of https://github.com/tu-usuario/diario
41c8532 Agregar resumen

$ cat diario.txt
Diario de estudio
Entrada 1: instale Git
Entrada 2: cree mi primera rama
Entrada 3: aprendi a clonar
Entrada 4: otra persona

$ git push
To https://github.com/tu-usuario/diario.git
   c563374..7940518  master -> master

$ cd ../otra-persona
$ git pull
From https://github.com/tu-usuario/diario
   072833b..7940518  master     -> origin/master
Updating 072833b..7940518
Fast-forward
 resumen.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 resumen.txt

$ git log --oneline
7940518 Revert "Agregar entrada 5 con dato incorrecto"
c563374 Agregar entrada 5 con dato incorrecto
293b92b Merge branch 'master' of https://github.com/tu-usuario/diario
41c8532 Agregar resumen
```

(Los identificadores serán distintos en tu repositorio. En este ejemplo el pull de la
otra persona también trae `resumen.txt` porque ella aún no tenía ese commit: el
`Fast-forward` incluye todo lo que le faltaba.)
