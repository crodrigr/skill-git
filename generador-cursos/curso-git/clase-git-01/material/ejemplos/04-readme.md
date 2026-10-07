# Ejemplo 04 — Crear y confirmar un README.md

## Estado inicial

Un repositorio local ya inicializado, sin `README.md` todavía (por ejemplo, el
repositorio del Ejemplo 02).

## Comandos

```bash
cat > README.md <<'EOF'
# Agenda de Contactos

Aplicacion de consola para guardar nombres y telefonos de contactos.

## Uso

Ejecutar `python3 main.py` desde la terminal.
EOF

git status
git add README.md
git commit -m "Agregar README con descripcion del proyecto"

git log --oneline
git show --stat HEAD
```

## Explicación paso a paso

1. Se crea `README.md` en la raíz del repositorio con un título, una descripción breve
   del proyecto y una sección de uso. El contenido exacto varía según el proyecto; lo
   importante es que explique qué es y cómo se usa.
2. `git status` muestra `README.md` como "Untracked": existe en la carpeta pero Git
   todavía no lo sigue.
3. `git add README.md` lo prepara en el área de staging, igual que cualquier otro
   archivo (Ejemplo 02).
4. `git commit -m "..."` lo confirma con un mensaje descriptivo, como a cualquier otro
   cambio.
5. `git log --oneline` y `git show --stat HEAD` confirman que el commit quedó
   registrado y qué archivo incluyó.

## Resultado esperado

```console
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
  README.md

nothing added to commit but untracked files present (use "git add" to track)

$ git add README.md
$ git commit -m "Agregar README con descripcion del proyecto"
[master (root-commit) 7bc97a3] Agregar README con descripcion del proyecto
 1 file changed, 7 insertions(+)
 create mode 100644 README.md

$ git log --oneline
7bc97a3 Agregar README con descripcion del proyecto

$ git show --stat HEAD
commit 7bc97a39ea4db26766e559f54dc1a2fae0c17f5a
Author: Ada Lovelace <ada@ejemplo.com>
Date:   Wed Oct 7 16:11:57 2026 -0500

    Agregar README con descripcion del proyecto

 README.md | 7 +++++++
 1 file changed, 7 insertions(+)
```

(El identificador del commit, `7bc97a3`, va a ser distinto en tu propio repositorio.)
