# 🧭 Guía paso a paso — De un sitio web local a GitHub

En esta guía vas a hacer, de principio a fin, el recorrido que usa cualquier persona que desarrolla con Git: **crear un proyecto en tu
computadora, publicarlo en GitHub y seguir enviando cambios**. Al terminar tenés un sitio web con su historial en GitHub.

> 🔁 **La idea es que la repitas.** Hacela una vez siguiendo cada paso. Después repetila desde cero con otro proyecto (te sugerimos
> uno al final) hasta que puedas hacerla sin mirar la guía.

## 🎯 Lo que vas a lograr

1. Crear un proyecto local con un sitio web sencillo.
2. Guardar su primera versión con Git.
3. Crear un repositorio vacío en GitHub.
4. Relacionar el proyecto local con ese repositorio.
5. Publicar tu proyecto.
6. Hacer un cambio y publicarlo.

## 🧰 Antes de empezar

| Qué necesitás | Cómo comprobarlo |
|---|---|
| **Git** instalado | En una terminal, `git --version` muestra un número de versión |
| Una **cuenta de GitHub** | Podés entrar a [github.com](https://github.com) con tu usuario |
| Un **editor de texto** (por ejemplo, VS Code) | Podés crear y guardar archivos |
| Un **navegador** | Podés abrir un archivo `.html` con doble clic |
| Una **terminal** | Terminal en Linux y macOS; Git Bash o PowerShell en Windows |

Git necesita saber **quién sos** para firmar tus commits. Comprobá si ya lo configuraste:

```bash
git config --global user.name
git config --global user.email
```

Si alguno de los dos no muestra nada, configuralos (una sola vez en cada computadora):

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu-correo@ejemplo.com"
```

> 💡 Usá en `user.email` el correo con el que creaste tu cuenta de GitHub (o el que GitHub te asigna para ocultar el tuyo): así
> tus commits se asocian a tu perfil.
>
> 📌 En esta guía, `tu-usuario` es **tu nombre de usuario de GitHub**. Reemplazalo siempre por el tuyo.

---

## 🪜 Parte 1 — El proyecto local

### Paso 1 — Crear la carpeta del proyecto

Elegí un lugar donde guardar tus proyectos (por ejemplo, una carpeta `proyectos`) y creá ahí la carpeta del sitio. Después, entrá en ella.

```bash
mkdir mi-sitio-web
cd mi-sitio-web
```

> 💡 Todos los comandos que siguen se ejecutan **dentro de `mi-sitio-web`**, salvo que se indique otra cosa. Si dudás de dónde estás,
> ejecutá `pwd` (muestra la carpeta actual).

### Paso 2 — Crear los archivos del sitio

Con tu editor, creá estos dos archivos dentro de `mi-sitio-web`, con **exactamente** esta estructura:

```text
mi-sitio-web/
├── index.html
└── css/
    └── estilos.css
```

Contenido de `index.html`:

```html
<!doctype html>
<html lang="es">
  <head>
    <meta charset="utf-8">
    <title>Mi sitio web</title>
    <link rel="stylesheet" href="css/estilos.css">
  </head>
  <body>
    <h1>Mi sitio web</h1>
    <p>Esta es mi primera página, publicada con Git y GitHub.</p>
  </body>
</html>
```

Contenido de `css/estilos.css`:

```css
body {
  font-family: sans-serif;
  margin: 2rem;
  background-color: #eff6ff;
}

h1 {
  color: #1e3a8a;
}
```

Guardá los dos archivos.

### Paso 3 — Probar el sitio en el navegador

Abrí `index.html` con doble clic (o arrastralo al navegador).

✅ **Lo que tenés que ver:** un título azul oscuro, «Mi sitio web», y un párrafo debajo, sobre un fondo celeste muy claro.

> 🧯 Si ves el título en negro y sin fondo, el CSS no se aplicó: revisá que la carpeta se llame `css`, que el archivo se llame
> `estilos.css` y que la línea `<link>` de `index.html` esté dentro de `<head>`.

### Paso 4 — Convertir la carpeta en un repositorio de Git

```bash
git init -b main
```

El `-b main` le pone `main` a la rama principal, que es el nombre que usa GitHub.

✅ **Resultado esperado**

```console
$ git init -b main
Initialized empty Git repository in /ruta/a/mi-sitio-web/.git/
```

> 📌 En otras partes del curso vas a ver `master`: es el mismo concepto con otro nombre. Lo importante es que uses **el mismo nombre
> en todos los pasos** de esta guía.

Comprobá el estado:

```bash
git status
```

```console
$ git status
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
    css/
    index.html

nothing added to commit but untracked files present (use "git add" to track)
```

Git ya ve tus dos archivos, pero todavía **no los sigue** (están «sin seguimiento»).

### Paso 5 — Preparar los archivos para el primer commit

```bash
git add .
git status
```

El punto significa «todo lo de esta carpeta».

✅ **Resultado esperado**

```console
$ git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
    new file:   css/estilos.css
    new file:   index.html
```

Los dos archivos aparecen en verde: están en el *staging* (listos para guardarse).

### Paso 6 — Guardar la primera versión (commit)

```bash
git commit -m "Primer commit: estructura inicial del sitio"
git log --oneline
```

✅ **Resultado esperado** (el código de siete caracteres será distinto en tu computadora)

```console
$ git commit -m "Primer commit: estructura inicial del sitio"
[main (root-commit) 772f8e5] Primer commit: estructura inicial del sitio
 2 files changed, 21 insertions(+)
 create mode 100644 css/estilos.css
 create mode 100644 index.html

$ git log --oneline
772f8e5 Primer commit: estructura inicial del sitio
```

🎉 Ya tenés tu primera versión guardada, **solo en tu computadora**.

---

## 🪜 Parte 2 — El repositorio en GitHub

### Paso 7 — Crear un repositorio vacío en GitHub

1. Entrá a [github.com](https://github.com) e iniciá sesión.
2. En la esquina superior derecha, hacé clic en el ícono **+** y elegí **New repository** (nuevo repositorio).
3. En **Repository name**, escribí `mi-sitio-web`.
4. Elegí la visibilidad: **Public** (cualquiera puede verlo) o **Private** (solo vos). Para practicar, cualquiera de las dos sirve.
5. **Dejá todo lo demás sin marcar**: sin README, sin `.gitignore` y sin licencia.
6. Hacé clic en **Create repository**.

> ⚠️ **No agregues un README al crearlo.** Tu proyecto local ya tiene su propio historial; si el repositorio de GitHub también arranca
> con un commit, los dos historiales no coinciden y Git no te deja publicar sin antes unirlos. GitHub, en su guía rápida, sugiere agregar un
> README porque ahí **parte de cero**; vos ya tenés un proyecto.
>
> 💡 La interfaz de GitHub cambia de vez en cuando: si un botón tiene otro texto o está en otro lugar, buscá la opción equivalente.

✅ **Lo que tenés que ver:** una página casi vacía, con un recuadro de instrucciones y una dirección parecida a
`https://github.com/tu-usuario/mi-sitio-web.git`. **Copiá esa dirección**: la necesitás en el paso siguiente.

### Paso 8 — Relacionar el proyecto local con el repositorio de GitHub

Volvé a la terminal, dentro de `mi-sitio-web`, y ejecutá (con **tu** dirección):

```bash
git remote add origin https://github.com/tu-usuario/mi-sitio-web.git
git remote -v
```

`origin` es el nombre habitual que se le da al repositorio remoto principal.

✅ **Resultado esperado**

```console
$ git remote -v
origin    https://github.com/tu-usuario/mi-sitio-web.git (fetch)
origin    https://github.com/tu-usuario/mi-sitio-web.git (push)
```

Una línea es para **traer** cambios (*fetch*) y otra para **publicar** (*push*).

> 💡 GitHub también ofrece una dirección SSH (`git@github.com:tu-usuario/mi-sitio-web.git`). Funciona igual, pero necesita una clave
> SSH configurada. En esta guía usamos la dirección **HTTPS**, que no necesita configuración previa.

### Paso 9 — Publicar tu proyecto (push)

```bash
git push -u origin main
```

Esto sube la rama `main` al remoto `origin`. La opción `-u` deja **recordada** la relación: más adelante alcanzará con escribir
`git push`.

**Cuando Git te pida la clave.** Con HTTPS, Git te pide un **usuario** y una **contraseña**:

- **Usuario:** tu nombre de usuario de GitHub.
- **Contraseña:** **no** es la de tu cuenta. GitHub eliminó el acceso con contraseña para Git: tenés que pegar un **token de acceso personal**.

Para crear un token (una sola vez):

1. En GitHub, hacé clic en tu foto de perfil y elegí **Settings**.
2. En la barra lateral izquierda, entrá en **Developer settings**.
3. En **Personal access tokens**, elegí **Fine-grained tokens** y hacé clic en **Generate new token**.
4. Poné un nombre y una fecha de vencimiento.
5. En **Repository access**, elegí **Only select repositories** y marcá `mi-sitio-web`.
6. En **Permissions**, dale a **Contents** el permiso **Read and write**.
7. Hacé clic en **Generate token** y **copiá el token**.

> ⚠️ **Tratá el token como una contraseña.** No lo compartas ni lo escribas en tus archivos. Guardalo en un administrador de contraseñas.
>
> 💡 Hay alternativas para no pegar el token cada vez: la herramienta **GitHub CLI** (`gh auth login`), un **administrador de
> credenciales** de Git o una **clave SSH**. Podés usar la que prefieras; esta guía funciona igual.

✅ **Resultado esperado** (en GitHub, Git muestra además líneas de progreso, como `Enumerating objects` o `Writing objects`)

```console
$ git push -u origin main
To https://github.com/tu-usuario/mi-sitio-web.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
```

### Paso 10 — Comprobar el resultado en GitHub

1. Volvé a la página de tu repositorio en GitHub y recargala (**F5**).
2. Tenés que ver los archivos `index.html` y la carpeta `css`.
3. Hacé clic en **commits** (o en el reloj con el número de commits) y buscá tu mensaje: «Primer commit: estructura inicial del sitio».

También podés comprobarlo desde la terminal:

```bash
git status -sb
```

```console
$ git status -sb
## main...origin/main
```

Que no aparezca nada más junto a `main...origin/main` significa que **tu rama local y la de GitHub están al día**.

🎉 Tu sitio ya está publicado en GitHub.

---

## 🪜 Parte 3 — Hacer un cambio y subirlo

Este es el ciclo que vas a repetir **todos los días**: *editar → revisar → guardar → publicar*.

### Paso 11 — Hacer un cambio en el sitio

Abrí `index.html` en tu editor y agregá, justo **antes** de `</body>`, esta sección:

```html
    <h2>Sobre mí</h2>
    <p>Estoy aprendiendo a crear sitios web.</p>
```

El archivo completo queda así:

```html
<!doctype html>
<html lang="es">
  <head>
    <meta charset="utf-8">
    <title>Mi sitio web</title>
    <link rel="stylesheet" href="css/estilos.css">
  </head>
  <body>
    <h1>Mi sitio web</h1>
    <p>Esta es mi primera página, publicada con Git y GitHub.</p>
    <h2>Sobre mí</h2>
    <p>Estoy aprendiendo a crear sitios web.</p>
  </body>
</html>
```

Guardá el archivo y recargá `index.html` en el navegador: tenés que ver el nuevo subtítulo y el párrafo.

### Paso 12 — Revisar qué cambió

```bash
git status
git diff
```

✅ **Resultado esperado**

```console
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
    modified:   index.html

no changes added to commit (use "git add" and/or "git commit -a")

$ git diff
diff --git a/index.html b/index.html
index de41ff3..05e90b4 100644
--- a/index.html
+++ b/index.html
@@ -8,5 +8,7 @@
   <body>
     <h1>Mi sitio web</h1>
     <p>Esta es mi primera página, publicada con Git y GitHub.</p>
+    <h2>Sobre mí</h2>
+    <p>Estoy aprendiendo a crear sitios web.</p>
   </body>
 </html>
```

Las líneas que empiezan con `+` son las **agregadas**. (Los códigos de la línea `index` pueden ser distintos en tu computadora.)

### Paso 13 — Guardar el cambio (commit)

```bash
git add index.html
git commit -m "Agrega la sección Sobre mí"
git status -sb
```

✅ **Resultado esperado**

```console
$ git commit -m "Agrega la sección Sobre mí"
[main 670b6df] Agrega la sección Sobre mí
 1 file changed, 2 insertions(+)

$ git status -sb
## main...origin/main [ahead 1]
```

`[ahead 1]` significa que tenés **un commit que todavía no está en GitHub**.

### Paso 14 — Publicar el cambio (push)

```bash
git push
```

Como en el paso 9 usaste `-u`, ahora alcanza con `git push`.

✅ **Resultado esperado**

```console
$ git push
To https://github.com/tu-usuario/mi-sitio-web.git
   772f8e5..670b6df  main -> main
```

### Paso 15 — Comprobar el cambio en GitHub

1. Recargá la página de tu repositorio.
2. Abrí `index.html`: tenés que ver la sección «Sobre mí».
3. Entrá en el historial de commits: ahora hay **dos**, y el más reciente es «Agrega la sección Sobre mí».

También desde la terminal:

```bash
git log --oneline
git status -sb
```

```console
$ git log --oneline
670b6df Agrega la sección Sobre mí
772f8e5 Primer commit: estructura inicial del sitio

$ git status -sb
## main...origin/main
```

🎉 Completaste la guía.

---

## ✅ Lista de comprobación

Marcá cada punto cuando lo hayas logrado:

- [ ] El sitio se ve bien en el navegador, con su estilo.
- [ ] `git log --oneline` muestra **dos** commits.
- [ ] `git remote -v` muestra tu repositorio de GitHub.
- [ ] En GitHub se ven `index.html` y la carpeta `css`.
- [ ] En GitHub, `index.html` incluye la sección «Sobre mí».
- [ ] `git status -sb` muestra `## main...origin/main`, sin nada más.

## 🧯 Si algo falla

| Lo que ves | Qué pasó | Qué hacer |
|---|---|---|
| `fatal: not a git repository` | Estás fuera de la carpeta del proyecto, o no hiciste `git init` | Entrá en `mi-sitio-web` (`cd mi-sitio-web`) y ejecutá `git init -b main` si hace falta |
| `error: src refspec main does not match any` y `failed to push some refs` | Intentaste publicar sin ningún commit, o tu rama no se llama `main` | Hacé el primer commit (pasos 5 y 6) y revisá el nombre de tu rama con `git branch` |
| `error: remote origin already exists.` | Ya habías ejecutado `git remote add origin …` | Para cambiar la dirección: `git remote set-url origin <dirección>` |
| `Authentication failed` o te rechaza la contraseña | Usaste la contraseña de tu cuenta, o el token no tiene permisos | Creá un token con permiso **Contents: Read and write** sobre el repositorio y pegalo como contraseña |
| `Repository not found` | La dirección está mal escrita (usuario o nombre del repositorio) | Verificá con `git remote -v` y corregí con `git remote set-url` |
| `! [rejected] main -> main (fetch first)` al publicar | El repositorio de GitHub ya tenía commits (por ejemplo, un README) y tu historial no los incluye | Es el motivo por el que no se agrega un README al crearlo. Más adelante en el curso aprendés a unir los dos historiales con `git pull` |
| `nothing to commit, working tree clean` | No hay cambios para guardar | Revisá que hayas **guardado** el archivo en el editor |
| `Your branch is ahead of 'origin/main'` | Tenés commits locales sin publicar | Ejecutá `git push` |

> 💡 Cuando un comando falle, **leé el mensaje completo**: casi siempre dice qué pasó y, muchas veces, qué comando probar.

## 🔁 Repetí la guía

Para aprender el ciclo hay que repetirlo. Hacé la guía **otra vez, desde el paso 1**, con un proyecto nuevo. Cambiá estos datos:

| En la guía | En tu repetición |
|---|---|
| Carpeta `mi-sitio-web` | `mi-portafolio` (o el nombre de tu proyecto) |
| Título «Mi sitio web» | El título de tu nuevo sitio |
| Mensaje del primer commit | Uno que describa tu proyecto |
| Sección «Sobre mí» | Una sección nueva: «Proyectos», «Contacto», «Hobbies»… |
| Repositorio `mi-sitio-web` | Uno nuevo en GitHub, con el nombre de tu proyecto |

**Desafío de la repetición.** Antes de mirar la guía, probá escribir de memoria los comandos de esta tabla en orden:

| # | Qué querés hacer | Comando |
|---|---|---|
| 1 | Convertir la carpeta en repositorio | `git init -b main` |
| 2 | Preparar todos los archivos | `git add .` |
| 3 | Guardar la primera versión | `git commit -m "…"` |
| 4 | Relacionar con GitHub | `git remote add origin <dirección>` |
| 5 | Publicar por primera vez | `git push -u origin main` |
| 6 | Revisar los cambios | `git status` y `git diff` |
| 7 | Guardar un cambio | `git add <archivo>` y `git commit -m "…"` |
| 8 | Publicar un cambio | `git push` |

## 🌿 Para seguir

Esta guía trabaja siempre sobre la rama `main`. En esta clase vas a aprender a trabajar con **ramas**: repetí el paso 11 al 14 creando antes una rama
(`git switch -c mejora-sobre-mi`), y publicala con `git push -u origin mejora-sobre-mi`.
