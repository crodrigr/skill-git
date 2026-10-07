# Soluciones — Quiz 01 (material docente)

No compartir este archivo con los estudiantes.

---

**1.** Respuesta correcta: **B**.
Justificación: el control de versiones registra cambios a lo largo del tiempo y
permite consultar el historial y volver atrás; no es un editor, ni un servicio de
backup automático, ni un lenguaje de programación.

**2.** Respuesta correcta: **B** (`git --version`).
Justificación: es el comando estándar para verificar la instalación y ver la versión
instalada; las otras opciones no son comandos reales de Git.

**3.** Respuesta correcta: **C** (`git config --global user.name "Tu Nombre"`).
Justificación: `git config --global` es el comando real para configurar valores que
aplican a todos los repositorios de la computadora.

**4.** Respuesta correcta: **D** (`git init`).
Justificación: `git init` es el comando que crea el repositorio (la carpeta `.git/`)
en la carpeta actual.

**5.** Respuesta esperada: `git status` va a mostrar `notas.txt` como **"Untracked
files"** (archivo no rastreado), porque todavía no se ejecutó `git add` sobre él.

**6.** Respuesta esperada: `git log --oneline` va a mostrar, de arriba hacia abajo (del
más reciente al más antiguo):

```text
<hash> Agregar validacion de datos
<hash> Agregar estructura inicial
```

**7.** Respuesta esperada: al estudiante le falta `git add notas.txt` antes de
`git commit`. Sin ese paso, el cambio en `notas.txt` nunca llegó al área de staging, así
que no hay nada nuevo para confirmar (Git va a avisar algo como "nothing to commit" o
confirmar un commit vacío si se fuerza). El área de staging es un paso obligatorio
entre modificar un archivo y confirmarlo.

**8.** Respuesta esperada (acepta variaciones de mensaje de commit, siempre que el
orden de comandos sea correcto):

```bash
git init
echo "# Mi proyecto" > README.md
git add README.md
git commit -m "Agregar README con descripcion del proyecto"
git log
```
