# Ejemplo 04 — Repositorio remoto y conexión

## Estado inicial

Un repositorio local con al menos un commit (por ejemplo, el del Ejemplo 02 de la
Clase 01). Una cuenta de GitHub, con un repositorio nuevo ya creado desde su interfaz
web (sin inicializarlo con README, para evitar historiales que no coinciden).

## Comandos

```bash
# En la practica/validacion de este material, el "remoto" es un repositorio
# --bare local (no una URL de GitHub real) - ver la nota de la explicacion conceptual.
# Para crear ese remoto de practica:
git init --bare ../remoto-practica.git

# Conectar el repositorio local con el remoto:
git remote add origin ../remoto-practica.git

# Verificar la conexion:
git remote -v
```

En tu propio proyecto con GitHub, el único cambio es la URL:

```bash
git remote add origin https://github.com/tu-usuario/tu-proyecto.git
git remote -v
```

## Explicación paso a paso

1. `git init --bare ../remoto-practica.git` crea un repositorio sin carpeta de
   trabajo: es el equivalente, para practicar, de lo que GitHub aloja del otro lado
   cuando creás un repositorio ahí.
2. `git remote add origin <url-o-ruta>` asocia esa ubicación con el nombre `origin`
   (el nombre convencional del remoto principal) dentro de tu repositorio local.
3. `git remote -v` imprime los remotos configurados: una línea para `fetch` (traer
   cambios) y otra para `push` (publicar cambios), normalmente iguales.
4. Si estuvieras usando GitHub real, el único paso distinto sería no crear el
   `--bare` local: en su lugar, crearías el repositorio desde la interfaz web de
   GitHub, y usarías la URL que GitHub te da (`https://github.com/...` o
   `git@github.com:...`) en el `git remote add`.

## Resultado esperado

```console
$ git init --bare ../remoto-practica.git
Initialized empty Git repository in .../remoto-practica.git/

$ git remote add origin ../remoto-practica.git
$ git remote -v
origin  ../remoto-practica.git (fetch)
origin  ../remoto-practica.git (push)
```

(Si usás una URL real de GitHub en vez de una ruta local, `git remote -v` muestra esa
URL en lugar de la ruta; el resto se comporta igual.)
