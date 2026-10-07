# Ejemplo 03 — Navegación del historial

## Estado inicial

El repositorio `mi-proyecto/` del Ejemplo 02, con dos commits ya confirmados
("Agregar README inicial" y "Agregar script inicial de la aplicacion").

## Comandos

```bash
# Ver el historial completo
git log

# Ver el detalle de un commit anterior (usando su identificador)
git show df7472d

# Ver el commit anterior al mas reciente, sin conocer su identificador
git show HEAD~1
```

## Explicación paso a paso

1. `git log` (sin `--oneline`) muestra el historial completo: identificador completo
   de cada commit, autor, fecha y mensaje, del más reciente al más antiguo.
2. `git show <identificador>` muestra el detalle de un commit puntual: su mensaje y
   qué archivos cambiaron (con `--stat`, un resumen; sin esa opción, el contenido línea
   por línea). Acá se usa el identificador corto `df7472d` que apareció en `git log`.
3. `HEAD` es una referencia al commit más reciente del historial; `HEAD~1` significa
   "un commit antes de HEAD". Es una forma de referirse a un commit anterior **sin
   necesidad de copiar su identificador**.
4. Ninguno de estos dos comandos modifica el repositorio: solo muestran información.

## Resultado esperado

```console
$ git log
commit 984b9cf8586fd6a62afa471488dbf64fda80f0fa
Author: Ada Lovelace <ada@ejemplo.com>
Date:   Wed Oct 7 16:10:42 2026 -0500

    Agregar script inicial de la aplicacion

commit df7472df4c007965f069037320a42b562f007cd1
Author: Ada Lovelace <ada@ejemplo.com>
Date:   Wed Oct 7 16:10:42 2026 -0500

    Agregar README inicial

$ git show df7472d --stat
commit df7472df4c007965f069037320a42b562f007cd1
Author: Ada Lovelace <ada@ejemplo.com>
Date:   Wed Oct 7 16:10:42 2026 -0500

    Agregar README inicial

 README.md | 1 +
 1 file changed, 1 insertion(+)

$ git show HEAD~1 --stat
commit df7472df4c007965f069037320a42b562f007cd1
Author: Ada Lovelace <ada@ejemplo.com>
Date:   Wed Oct 7 16:10:42 2026 -0500

    Agregar README inicial

 README.md | 1 +
 1 file changed, 1 insertion(+)
```

(`git show df7472d` y `git show HEAD~1` muestran el mismo commit en este ejemplo,
porque `df7472d` es justamente el commit anterior al más reciente. Los identificadores
van a ser distintos en tu propio repositorio.)
