# 🧭 Guía 02 — Solución del taller paso a paso: trabajo en equipo con ramas

Esta guía resuelve el [Taller 02 — Trabajo en equipo con ramas](taller-02-equipo-con-ramas.md) de principio a fin. Está escrita para que **cada persona del grupo**
pueda seguirla desde su lugar: la persona propietaria del repositorio y las personas colaboradoras.

> 🔁 **La idea es que la repitas.** Hacé el taller con tu grupo; después repetí esta guía (podés hacerlo con otra tarea o en otro grupo) hasta que el ciclo
> *rama → cambios → integrar → publicar* te salga sin mirarla.

## 🎯 Lo que vas a lograr

1. Invitar colaboradores a un repositorio de GitHub y aceptar una invitación.
2. Clonar un repositorio.
3. Crear una rama para tu tarea a partir de `master` actualizada.
4. Integrar tu rama en `master` en tu computadora y publicarla.
5. Resolver un conflicto al integrar tu rama.
6. Resolver un conflicto cuando GitHub rechaza tu publicación.
7. Borrar las ramas que ya integraste.

## 👥 El equipo del ejemplo

Para que los ejemplos sean concretos, la guía sigue a un grupo ficticio. Reemplazá los nombres por los de tu grupo.

| Persona | Rol | Tarea | Rama |
|---|---|---|---|
| Ana | Propietaria del repositorio (`ana-ejemplo` en GitHub) | 1. Menú de navegación | `tarea/menu` |
| Bruno | Colaborador | 2. Sección «Proyectos» | `tarea/proyectos` |
| Carla | Colaboradora | 3. Sección «Contacto» | `tarea/contacto` |
| Diego | Colaborador | 5. Estilos del texto (subtítulos) | `tarea/estilos-titulos` |
| Elena | Colaboradora | 5. Estilos del texto (párrafos) | `tarea/estilos-parrafos` |
| Facundo | Colaborador | 4. Pie de página | `tarea/pie` |

La dirección del repositorio del ejemplo es `https://github.com/ana-ejemplo/mi-sitio-web.git`. **Usá la de tu grupo.**

> 📌 **Resultados esperados.** Las salidas de los comandos de esta guía se comprobaron con Git 2.34 sobre un repositorio de prueba. Los códigos de siete caracteres
> (como `c757e81`) serán **distintos en tu computadora**, y al trabajar con GitHub vas a ver además algunas líneas de progreso (`Enumerating objects…`,
> `Writing objects…`). Los textos de la interfaz de GitHub pueden cambiar con el tiempo: si un botón tiene otro nombre, buscá la opción equivalente.

---

## 🧰 Parte 0 — Preparación (todas las personas)

### Paso 1 — Comprobar tu identidad en Git

```bash
git config --global user.name
git config --global user.email
```

Si alguno no muestra nada, configuralo como en la Guía 01:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu-correo@ejemplo.com"
```

### Paso 2 — Decirle a Git cómo combinar al traer cambios

Cuando traigas cambios de GitHub con `git pull` y tu `master` tenga commits que GitHub no tiene (y al revés), Git necesita saber **cómo combinarlos**. Si no se
lo dijiste, se detiene con este mensaje:

```console
hint: You have divergent branches and need to specify how to reconcile them.
...
fatal: Need to specify how to reconcile divergent branches.
```

En este taller combinamos con una **fusión** (*merge*). Configuralo **una sola vez** en tu computadora:

```bash
git config --global pull.rebase false
```

> 💡 Hay otras formas de combinar (por ejemplo, *rebase*), pero en esta clase usamos solo la fusión, que es la que ya conocés de `git merge`.

### Paso 3 (opcional) — Usar VS Code para los mensajes de fusión

Cuando Git hace un commit de fusión, **abre un editor** para que confirmes el mensaje. Si nunca configuraste uno, puede abrirse un editor de terminal (como Vim o
nano), que confunde la primera vez. Para que use VS Code:

```bash
git config --global core.editor "code --wait"
```

Con esto, cuando Git abra el mensaje en VS Code, **guardá y cerrá esa pestaña** para que el comando termine.

> 🧯 Si se te abrió **Vim** (pantalla negra, con líneas que empiezan con `~`): escribí `:wq` y apretá **Enter** para guardar y salir. Si se abrió **nano**:
> apretá **Ctrl+O**, **Enter** y después **Ctrl+X**.

---

## 🪜 Parte 1 — Colaboradores del repositorio

### Paso 4 — Invitar a los colaboradores (persona propietaria)

Lo hace **solo Ana**, desde su cuenta de GitHub:

1. Entrá a tu repositorio `mi-sitio-web` en GitHub.
2. Hacé clic en **Settings** (configuración del repositorio, en la barra superior del repositorio).
3. En la barra lateral, buscá la sección **Access** y entrá en **Collaborators**. GitHub puede pedirte que confirmes tu contraseña.
4. Hacé clic en **Add people**.
5. Escribí el **nombre de usuario** (o el correo) de una persona del grupo, elegila de la lista y confirmá con **Add … to this repository**.
6. Repetí para cada integrante.

✅ **Lo que tenés que ver:** la lista de colaboradores con cada persona invitada como **pendiente** (*pending invite*) hasta que acepte.

> 📌 En un repositorio de una cuenta personal, los colaboradores pueden **leer y publicar** en el repositorio: es lo que necesitan para el taller.

### Paso 5 — Aceptar la invitación (colaboradores)

Cada integrante:

1. Revisá tu **correo** o las **notificaciones** de GitHub (la campana, arriba a la derecha): hay una invitación de `ana-ejemplo`.
2. Abrila y hacé clic en **Accept invitation**.

> 💡 También podés entrar directamente a `https://github.com/ana-ejemplo/mi-sitio-web/invitations` (con el usuario y el repositorio de tu grupo).

✅ **Lo que tenés que ver:** el repositorio de Ana abierto, ahora con acceso de colaborador. Ana, en **Collaborators**, ya no ve la invitación como pendiente.

### Paso 6 — Comprobar que podés publicar (colaboradores)

Para publicar en GitHub con HTTPS vas a necesitar, como en la Guía 01, tu **usuario** y un **token** en lugar de la contraseña.

> ⚠️ **El repositorio no es tuyo.** Un *token de grano fino* (*fine-grained*) se crea para los repositorios de una cuenta; si el tuyo no te deja publicar en el
> repositorio de Ana (`Permission denied` o `403`), usá una de estas alternativas:
>
> - iniciá sesión con **GitHub CLI**: `gh auth login`;
> - usá el **administrador de credenciales** de Git (en Windows viene con Git y abre el navegador para que inicies sesión);
> - o creá un **token clásico** (*Tokens (classic)*) con el permiso **repo**.
>
> Tratá cualquier token como una contraseña: no lo compartas ni lo escribas en tus archivos.

---

## 🪜 Parte 2 — Clonar el repositorio

### Paso 7 — Clonar (colaboradores)

**Clonar** es descargar una copia completa del repositorio, con todo su historial, y dejarla ya relacionada con GitHub.

1. En la página del repositorio de Ana, hacé clic en el botón verde **Code**, elegí la pestaña **HTTPS** y copiá la dirección.
2. Con el explorador de archivos, elegí la carpeta donde guardás tus proyectos y **abrí una terminal ahí**: en Ubuntu, clic derecho y **Abrir en una terminal**; en
   Windows, clic derecho y **Open Git Bash here**. (También sirve abrir esa carpeta en VS Code y usar **Terminal → Nueva terminal**.)
3. Escribí:

```bash
git clone https://github.com/ana-ejemplo/mi-sitio-web.git
```

✅ **Resultado esperado**

```console
$ git clone https://github.com/ana-ejemplo/mi-sitio-web.git
Cloning into 'mi-sitio-web'...
done.
```

Git crea la carpeta `mi-sitio-web`. **Abrila en VS Code** (**Archivo → Abrir carpeta…**) y abrí una terminal nueva (**Terminal → Nueva terminal**): ya queda
ubicada dentro del proyecto. **Desde ahora, todos los comandos se escriben en esa terminal.**

> 📌 **Ana no clona.** Ella ya tiene el proyecto en su computadora desde la Guía 01: trabaja en esa carpeta.

### Paso 8 — Comprobar el clon

```bash
git log --oneline
git remote -v
git branch -a
```

✅ **Resultado esperado**

```console
$ git log --oneline
0ffcec4 Agrega la sección Sobre mí
26c02a0 Primer commit: estructura inicial del sitio

$ git remote -v
origin  https://github.com/ana-ejemplo/mi-sitio-web.git (fetch)
origin  https://github.com/ana-ejemplo/mi-sitio-web.git (push)

$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```

- Tenés **el mismo historial** que Ana.
- `origin` ya apunta al repositorio de GitHub: no hace falta `git remote add`.
- `remotes/origin/master` es la copia que tu Git guarda de la rama `master` de GitHub.

---

## 🪜 Parte 3 — Tu tarea, en tu rama

Desde acá, **todas las personas** (también Ana) siguen el mismo ciclo. Lo mostramos con **Bruno** y su tarea «Proyectos».

### Paso 9 — Actualizar `master` antes de empezar

```bash
git switch master
git pull
```

✅ **Resultado esperado** (si nadie publicó todavía)

```console
$ git switch master
Already on 'master'
Your branch is up to date with 'origin/master'.

$ git pull
Already up to date.
```

`git pull` **trae** lo que haya nuevo en GitHub y lo **integra** en tu rama actual. Si alguien ya publicó, vas a ver los archivos que cambiaron.

### Paso 10 — Crear la rama de la tarea

```bash
git switch -c tarea/proyectos
git branch
```

✅ **Resultado esperado**

```console
$ git switch -c tarea/proyectos
Switched to a new branch 'tarea/proyectos'

$ git branch
  master
* tarea/proyectos
```

El asterisco marca la rama en la que estás. Todo lo que guardes ahora queda en `tarea/proyectos`, **sin tocar `master`**.

### Paso 11 — Hacer la tarea

Bruno abre `index.html` y agrega, justo **antes** de `</body>`:

```html
    <h2>Proyectos</h2>
    <p>Mi primer proyecto es este sitio web.</p>
```

Guarda el archivo y lo prueba en el navegador.

> 💡 **Cada tarea, lo suyo.** Carla agrega su sección «Contacto» en el mismo lugar; Facundo, su `<footer>`; Ana, un `<nav>` debajo del `<h1>`; Diego y Elena
> agregan reglas al final de `css/estilos.css`. Todos siguen los mismos pasos.

### Paso 12 — Guardar el trabajo en la rama

```bash
git status
git add index.html
git commit -m "Agrega la sección Proyectos"
```

✅ **Resultado esperado**

```console
$ git commit -m "Agrega la sección Proyectos"
[tarea/proyectos c757e81] Agrega la sección Proyectos
 1 file changed, 2 insertions(+)
```

Si tu tarea es larga, hacé **varios commits** en la rama: uno por cada avance que funcione.

### Paso 13 (opcional) — Publicar la rama como respaldo

Si querés que tu rama quede guardada en GitHub mientras trabajás (por ejemplo, para seguir desde otra computadora):

```bash
git push -u origin tarea/proyectos
```

✅ **Resultado esperado**

```console
$ git push -u origin tarea/proyectos
To https://github.com/ana-ejemplo/mi-sitio-web.git
 * [new branch]      tarea/proyectos -> tarea/proyectos
Branch 'tarea/proyectos' set up to track remote branch 'tarea/proyectos' from 'origin'.
```

> 📌 Publicar la rama **no la integra**: `master` sigue igual. GitHub puede mostrarte un botón para crear un *pull request*; en este taller **no lo usamos**.

---

## 🪜 Parte 4 — Integrar tu rama en `master` y publicar

### Paso 14 — Volver a `master` y actualizarla

```bash
git switch master
git pull
```

Este paso es **clave**: integrás tu rama sobre la versión **más nueva** de `master`, con lo que ya publicaron tus compañeros.

### Paso 15 — Integrar tu rama

```bash
git merge tarea/proyectos
```

✅ **Resultado esperado** (cuando nadie más cambió `master`: Bruno fue el primero)

```console
$ git merge tarea/proyectos
Updating 0ffcec4..c757e81
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)
```

*Fast-forward* (avance rápido) significa que `master` no tenía nada nuevo: Git solo **adelantó** `master` hasta tu último commit.

### Paso 16 — Publicar `master`

```bash
git push
```

✅ **Resultado esperado**

```console
$ git push
To https://github.com/ana-ejemplo/mi-sitio-web.git
   0ffcec4..c757e81  master -> master
```

💬 **Avisá al grupo** que publicaste: el resto tiene que hacer `git pull` antes de integrar.

### Paso 17 — Borrar la rama integrada

```bash
git branch -d tarea/proyectos
```

```console
$ git branch -d tarea/proyectos
Deleted branch tarea/proyectos (was c757e81).
```

Si la habías publicado en el paso 13, borrala también en GitHub:

```bash
git push origin --delete tarea/proyectos
```

```console
$ git push origin --delete tarea/proyectos
To https://github.com/ana-ejemplo/mi-sitio-web.git
 - [deleted]         tarea/proyectos
```

> 💡 `git branch -d` solo borra una rama **que ya está integrada**. Si Git se niega (`error: The branch … is not fully merged`), es que te falta integrarla:
> no la fuerces.

---

## 🪜 Parte 5 — Conflicto al integrar tu rama

Ahora le toca a **Carla**. Su rama `tarea/contacto` agrega la sección «Contacto» **en el mismo lugar** donde Bruno agregó «Proyectos».

### Paso 18 — Actualizar `master` e integrar

```bash
git switch master
git pull
git merge tarea/contacto
```

✅ **Resultado esperado**

```console
$ git pull
From https://github.com/ana-ejemplo/mi-sitio-web
   0ffcec4..c757e81  master     -> origin/master
Updating 0ffcec4..c757e81
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)

$ git merge tarea/contacto
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

El `git pull` trajo el trabajo de Bruno. Al integrar `tarea/contacto`, Git encontró que **las dos ramas cambiaron las mismas líneas** y no puede decidir solo:
hay un **conflicto**.

### Paso 19 — Ver qué está en conflicto

```bash
git status
```

```console
$ git status
On branch master
Your branch is up to date with 'origin/master'.

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
    both modified:   index.html
```

*both modified* significa que las dos ramas modificaron `index.html`.

### Paso 20 — Entender las marcas de conflicto

Abrí `index.html`. Git dejó las dos versiones, separadas por **marcas**:

```html
    <h2>Sobre mí</h2>
    <p>Estoy aprendiendo a crear sitios web.</p>
<<<<<<< HEAD
    <h2>Proyectos</h2>
    <p>Mi primer proyecto es este sitio web.</p>
=======
    <h2>Contacto</h2>
    <p>Escribime a ana@ejemplo.com.</p>
>>>>>>> tarea/contacto
  </body>
```

| Marca | Significa |
|---|---|
| `<<<<<<< HEAD` | Empieza lo que ya estaba en tu rama actual (`master`, con el trabajo de Bruno) |
| `=======` | Separa las dos versiones |
| `>>>>>>> tarea/contacto` | Termina lo que trae la rama que estás integrando (el trabajo de Carla) |

### Paso 21 — Resolver el conflicto

Hay que dejar el archivo **como tiene que quedar** y **borrar las tres marcas**. En este caso, el grupo quiere **las dos secciones**:

```html
    <h2>Sobre mí</h2>
    <p>Estoy aprendiendo a crear sitios web.</p>
    <h2>Proyectos</h2>
    <p>Mi primer proyecto es este sitio web.</p>
    <h2>Contacto</h2>
    <p>Escribime a ana@ejemplo.com.</p>
  </body>
```

> 💡 **En VS Code** aparecen botones sobre el conflicto: **Accept Current Change** (dejar lo de `HEAD`), **Accept Incoming Change** (dejar lo que llega),
> **Accept Both Changes** (dejar las dos) y **Compare Changes**. Acá sirve **Accept Both Changes**. Revisá siempre el resultado: a veces hay que retocarlo a mano.

Guardá el archivo y **probalo en el navegador**: tienen que verse las dos secciones.

> ⚠️ Antes de seguir, buscá en el archivo `<<<<<<<`, `=======` y `>>>>>>>`: **no tiene que quedar ninguna**.

### Paso 22 — Marcar como resuelto y terminar la fusión

```bash
git add index.html
git status
git commit
```

✅ **Resultado esperado**

```console
$ git status
On branch master
Your branch is up to date with 'origin/master'.

All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:
    modified:   index.html

$ git commit
[master 0bc94ad] Merge branch 'tarea/contacto'
```

Al hacer `git commit` **sin `-m`**, Git abre el editor con un mensaje ya escrito, `Merge branch 'tarea/contacto'`. **Guardá y cerrá** (paso 3) para terminar.

### Paso 23 — Publicar y mirar el historial

```bash
git push
git log --oneline --graph
```

✅ **Resultado esperado**

```console
$ git push
To https://github.com/ana-ejemplo/mi-sitio-web.git
   c757e81..0bc94ad  master -> master

$ git log --oneline --graph
*   0bc94ad Merge branch 'tarea/contacto'
|\
| * b7c479c Agrega la sección Contacto
* | c757e81 Agrega la sección Proyectos
|/
* 0ffcec4 Agrega la sección Sobre mí
* 26c02a0 Primer commit: estructura inicial del sitio
```

El gráfico muestra las dos líneas de trabajo y el **commit de fusión** que las une. Después, Carla borra su rama (paso 17).

> 🧯 **Si te arrepentís en medio de un conflicto**, `git merge --abort` deja todo como estaba antes del `git merge`. Podés volver a intentarlo cuando quieras.

---

## 🪜 Parte 6 — GitHub rechaza tu publicación

Mientras tanto, **Diego** terminó su tarea (color de los subtítulos, al final de `css/estilos.css`) y la integró en su `master`, **pero sin hacer `git pull`
antes** (se salteó el paso 14). Al publicar:

### Paso 24 — El rechazo

```console
$ git push
To https://github.com/ana-ejemplo/mi-sitio-web.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/ana-ejemplo/mi-sitio-web.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
```

GitHub tiene commits (los de Bruno y Carla) que Diego no tiene. **No es un error grave**: Git te protege para que no borres el trabajo de nadie.

> 🚫 **No uses `git push --force`.** Publicaría tu `master` encima del de GitHub y **borraría** el trabajo de Bruno y Carla.

### Paso 25 — Traer e integrar los cambios del remoto

```bash
git pull
```

✅ **Resultado esperado** (sin conflicto: Diego tocó el CSS y los demás, el HTML)

```console
$ git pull
From https://github.com/ana-ejemplo/mi-sitio-web
   0ffcec4..0bc94ad  master     -> origin/master
Merge made by the 'ort' strategy.
 index.html | 4 ++++
 1 file changed, 4 insertions(+)
```

Git hizo un **commit de fusión** automático (te puede abrir el editor para confirmar el mensaje: guardá y cerrá). Ahora publicá:

```bash
git push
```

```console
$ git push
To https://github.com/ana-ejemplo/mi-sitio-web.git
   0bc94ad..0269ff9  master -> master
```

### Paso 26 — Rechazo **con** conflicto

**Elena** agregó, también al final de `css/estilos.css`, la altura de línea de los párrafos, y le pasó lo mismo que a Diego: su publicación fue rechazada. Pero al hacer
`git pull`, como **los dos agregaron reglas en el mismo lugar**, hay conflicto:

```console
$ git pull
From https://github.com/ana-ejemplo/mi-sitio-web
   0bc94ad..0269ff9  master     -> origin/master
Auto-merging css/estilos.css
CONFLICT (content): Merge conflict in css/estilos.css
Automatic merge failed; fix conflicts and then commit the result.
```

El final de `css/estilos.css` queda así:

```css
h1 {
  color: #1e3a8a;
}

<<<<<<< HEAD
p {
  line-height: 1.6;
=======
h2 {
  color: #2563eb;
>>>>>>> 0269ff9a7d9a1d0bb5f5150bf490035e40a51ff4
}
```

> ⚠️ **Mirá la llave final `}`.** Quedó **fuera** de las marcas porque las dos versiones la tenían igual. Si solo borrás las marcas, te queda una regla sin cerrar.
> Por eso, revisá siempre que el resultado tenga sentido, no solo que no queden marcas.

La resolución correcta deja **las dos reglas completas**:

```css
h1 {
  color: #1e3a8a;
}

h2 {
  color: #2563eb;
}

p {
  line-height: 1.6;
}
```

Después:

```bash
git add css/estilos.css
git commit
git push
```

✅ **Resultado esperado** (el mensaje de la fusión lo propone Git)

```console
$ git commit
[master 3c37d6b] Merge branch 'master' of https://github.com/ana-ejemplo/mi-sitio-web

$ git push
To https://github.com/ana-ejemplo/mi-sitio-web.git
   0269ff9..3c37d6b  master -> master
```

---

## 🪜 Parte 7 — Cierre del taller

### Paso 27 — Todas las personas se actualizan

Cuando el grupo terminó de integrar, **cada persona** actualiza su `master` y limpia las ramas remotas que ya no existen:

```bash
git switch master
git pull
git fetch --prune
```

`git fetch --prune` borra de tu computadora la copia de las ramas que alguien ya eliminó en GitHub.

### Paso 28 — Comprobar el resultado

```bash
git log --oneline --graph --all
git branch -a
git status -sb
```

✅ **Lo que tenés que ver**

- En el gráfico, un commit de cada tarea y los commits de fusión que las unen.
- En `git branch -a`, solo `master` y `remotes/origin/master` (sin ramas `tarea/…`).
- `git status -sb` muestra `## master...origin/master`, sin nada más.
- En el navegador, `index.html` muestra el menú, «Sobre mí», «Proyectos», «Contacto» y el pie de página, con los estilos de Diego y de Elena.
- En GitHub, en el historial de commits, aparecen los commits de todas las personas.

---

## ✅ Lista de comprobación

Marcá cada punto cuando lo hayas logrado:

- [ ] Estoy en la lista de colaboradores del repositorio (o invité a todo mi grupo, si soy la persona propietaria).
- [ ] Cloné el repositorio y `git remote -v` muestra el repositorio del grupo.
- [ ] Hice mi tarea en una rama `tarea/…` creada desde `master` actualizada.
- [ ] Integré mi rama en `master` en mi computadora y la publiqué.
- [ ] Resolví (o ayudé a resolver) al menos un conflicto, sin dejar marcas.
- [ ] Borré mi rama local y, si la había publicado, la remota.
- [ ] `git status -sb` muestra `## master...origin/master`.

## 🧯 Si algo falla

| Lo que ves | Qué pasó | Qué hacer |
|---|---|---|
| No encontrás la invitación | Puede estar en el correo o en las notificaciones de GitHub | Entrá a `https://github.com/<propietaria>/<repositorio>/invitations` |
| `Permission denied` o `403` al publicar | No aceptaste la invitación, o tu token no tiene acceso al repositorio | Aceptá la invitación; probá `gh auth login`, el administrador de credenciales o un token clásico con permiso **repo** (paso 6) |
| `fatal: destination path 'mi-sitio-web' already exists and is not an empty directory.` | Ya existe una carpeta con ese nombre donde clonás | Cloná en otra carpeta o con otro nombre: `git clone <dirección> otro-nombre` |
| `fatal: Need to specify how to reconcile divergent branches.` | Git no sabe cómo combinar al hacer `git pull` | `git config --global pull.rebase false` (paso 2) y repetí `git pull` |
| `! [rejected] master -> master (fetch first)` | Alguien publicó antes que vos | `git pull`, resolvé si hay conflicto, y `git push` (parte 6) |
| `CONFLICT (content): Merge conflict in …` | Dos ramas cambiaron las mismas líneas | Resolvé las marcas, `git add` y `git commit` (partes 5 y 6) |
| `error: Committing is not possible because you have unmerged files.` | Intentaste hacer commit sin marcar el conflicto como resuelto | Resolvé el archivo y hacé `git add <archivo>` antes del commit |
| Se abrió una pantalla extraña con `~` al hacer commit | Git abrió el editor Vim para el mensaje de fusión | Escribí `:wq` y **Enter**; configurá VS Code (paso 3) |
| `error: The branch 'tarea/…' is not fully merged.` | Querés borrar una rama que todavía no integraste | Integrala primero (parte 4); no uses `-D` salvo que quieras descartar ese trabajo |
| La página se ve mal después de resolver | Quedó una marca o una etiqueta sin cerrar | Buscá `<<<<<<<`, `=======` y `>>>>>>>`, y revisá que cada etiqueta y cada llave estén cerradas |
| Hiciste commits en `master` sin querer | Te olvidaste de crear la rama | Si todavía no publicaste: `git switch -c tarea/<nombre>` (la rama nueva se queda con los commits) y avisá al docente para volver `master` atrás |

## 🗺️ Resumen de comandos

| # | Qué querés hacer | Comando |
|---|---|---|
| 1 | Configurar la fusión al traer cambios (una vez) | `git config --global pull.rebase false` |
| 2 | Clonar el repositorio | `git clone <dirección>` |
| 3 | Actualizar `master` | `git switch master` y `git pull` |
| 4 | Crear la rama de la tarea | `git switch -c tarea/<nombre>` |
| 5 | Guardar el trabajo | `git add <archivo>` y `git commit -m "…"` |
| 6 | Publicar la rama (opcional) | `git push -u origin tarea/<nombre>` |
| 7 | Integrar la rama en `master` | `git switch master`, `git pull` y `git merge tarea/<nombre>` |
| 8 | Terminar una fusión con conflicto | Resolver, `git add <archivo>` y `git commit` |
| 9 | Cancelar una fusión con conflicto | `git merge --abort` |
| 10 | Publicar `master` | `git push` |
| 11 | Borrar la rama | `git branch -d tarea/<nombre>` y `git push origin --delete tarea/<nombre>` |
| 12 | Ver el historial con las ramas | `git log --oneline --graph --all` |

## 🔁 Repetí la guía

Para afianzar el ciclo, repetilo con una **segunda ronda** de tareas, cambiando de rol:

| En la primera ronda | En la repetición |
|---|---|
| Una tarea por persona | Cada persona toma la tarea de otra y la mejora |
| La propietaria es Ana | Otra persona del grupo crea un repositorio nuevo (con la Guía 01) e invita al resto |
| Los conflictos aparecieron solos | Provóquenlos a propósito: dos personas cambian el texto del mismo `<h1>` |
| Integraron en cualquier orden | Acuerden un orden y comparen: ¿aparecen menos rechazos? |

**Desafío de la repetición.** Antes de mirar la guía, probá escribir de memoria la secuencia completa para una tarea nueva, desde `git switch master` hasta
`git branch -d`.
