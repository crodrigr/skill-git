# Ejemplo 03 — Push: commits y primer envío de una rama nueva

## Estado inicial

Tu copia de `diario` sincronizada con el remoto, en la rama `master`, con
`pull.rebase false` configurado. El remoto es un repositorio `--bare` local en lugar de
GitHub real.

## Comandos

```bash
# 1. Un commit nuevo y su envío
echo "Entrada 3: aprendi a clonar" >> diario.txt
git commit -am "Agregar entrada 3"
git push

# 2. Una rama nueva: el primer push necesita enlazarla
git switch -c entrada-viaje
echo "Viaje: Git en el tren" > viaje.txt
git add viaje.txt
git commit -m "Agregar nota de viaje"
git push                                   # Git indica qué falta
git push -u origin entrada-viaje           # primer envío con enlace
git branch -vv                             # ver el enlace con la rama remota
```

## Explicación paso a paso

1. `git commit -am` confirma el cambio en tu repositorio local; en este punto el
   remoto no sabe nada. `git push` publica el commit: la línea `a17a2c4..e3d9824`
   indica el rango de commits enviados.
2. `git switch -c entrada-viaje` crea y cambia a una rama nueva, y se confirma un
   commit en ella (Clase 02).
3. `git push` falla con `The current branch entrada-viaje has no upstream branch`: la
   rama existe solo en tu copia y Git no sabe a qué rama remota enviarla. El propio
   mensaje sugiere el comando.
4. `git push -u origin entrada-viaje` crea la rama en el remoto (`[new branch]`) y la
   enlaza con la local (`-u`, abreviatura de `--set-upstream`). Desde ahora basta con
   `git push` y `git pull` en esa rama.
5. `git branch -vv` muestra el enlace: `[origin/entrada-viaje]` junto a la rama local.

## Resultado esperado

```console
$ git push
To https://github.com/tu-usuario/diario.git
   a17a2c4..e3d9824  master -> master

$ git push
fatal: The current branch entrada-viaje has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin entrada-viaje

$ git push -u origin entrada-viaje
To https://github.com/tu-usuario/diario.git
 * [new branch]      entrada-viaje -> entrada-viaje
Branch 'entrada-viaje' set up to track remote branch 'entrada-viaje' from 'origin'.

$ git branch -vv
* entrada-viaje 4e30d52 [origin/entrada-viaje] Agregar nota de viaje
  master        e3d9824 [origin/master] Agregar entrada 3
```

(Con un remoto real verás además líneas de progreso como `Enumerating objects...`.
Los identificadores serán distintos en tu repositorio.)
