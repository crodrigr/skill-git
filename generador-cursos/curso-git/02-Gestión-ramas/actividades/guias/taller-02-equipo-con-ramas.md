# 🛠️ Taller 02 — Trabajo en equipo con ramas sobre un repositorio compartido

> 👥 **En grupos de 5 o 6 personas** · ⏱️ **Duración estimada:** 2 horas · 🧭 **Parte de:** la [Guía 01 — De un sitio web local a GitHub](guia-01-sitio-web-a-github.md)

## 🎯 Objetivo

Trabajar **en equipo sobre un mismo repositorio de GitHub**: invitar colaboradores, clonar el proyecto, desarrollar cada tarea en **su propia rama**, integrarla en
`master` **en tu computadora** y publicarla, resolviendo los **conflictos** que aparezcan cuando dos personas tocan el mismo archivo.

Al terminar, el equipo tiene un sitio web con el aporte de todas las personas y un historial que muestra cómo se integró cada rama.

> 📌 **Sin pull requests.** En este taller no se usan los *pull requests* de GitHub: cada persona integra su rama en `master` **en su repositorio local**
> (`git merge`) y después publica `master` (`git push`). Los pull requests se ven más adelante.

## 🌍 Contexto

Una persona del grupo ya tiene el repositorio `mi-sitio-web` que creó en la **Guía 01**: una página `index.html` con su hoja `css/estilos.css`, publicada en
GitHub en la rama `master`. Ese repositorio pasa a ser **el proyecto del equipo**. El resto del grupo se suma como **colaborador** y cada persona agrega una
mejora.

Las tareas están pensadas para que **varias personas modifiquen los mismos archivos**: los conflictos no son un error del taller, **son parte de lo que hay
que aprender a resolver**.

## 👤 Roles

| Rol | Quién | Responsabilidades |
|---|---|---|
| **Persona propietaria** | Quien tiene el repositorio de la Guía 01 | Invita al resto como colaboradores; también hace una tarea, en su rama, como cualquier integrante |
| **Integrantes colaboradores** | El resto del grupo (4 o 5 personas) | Aceptan la invitación, clonan el repositorio y hacen su tarea |
| **Todo el grupo** | Todas las personas | Acuerdan el orden de integración, se avisan cuando publican y se ayudan con los conflictos |

> 💡 Si nadie terminó la Guía 01, una persona la hace rápidamente (partes 1 y 2) antes de empezar el taller.

## 📋 Consignas

### 1️⃣ Colaboradores del repositorio

- La persona propietaria **invita a cada integrante** como colaborador del repositorio `mi-sitio-web` desde la configuración del repositorio en GitHub.
- Cada integrante **acepta la invitación**.
- El grupo comprueba que la lista de colaboradores esté completa.

### 2️⃣ Clonar el repositorio

- Cada integrante **clona** el repositorio en su computadora y comprueba que tiene el mismo historial que GitHub.
- La persona propietaria trabaja sobre el proyecto que ya tiene en su computadora (no hace falta que lo clone).

### 3️⃣ Tareas

Repartan **una tarea por persona**. En un grupo de 5, la tarea 6 queda como desafío opcional (o la toma quien termine primero).

| # | Tarea | Qué hay que lograr | Archivos que se tocan |
|---|---|---|---|
| 1 | **Menú de navegación** | Un menú, debajo del título principal, con enlaces a las secciones de la página | `index.html` |
| 2 | **Sección «Proyectos»** | Una sección nueva, al final de la página, con un subtítulo y la descripción de al menos un proyecto | `index.html` |
| 3 | **Sección «Contacto»** | Una sección nueva, al final de la página, con un subtítulo y una forma de contacto (ficticia) | `index.html` |
| 4 | **Pie de página** | Un pie de página con el año y el nombre del equipo | `index.html` |
| 5 | **Estilos del texto** | Color para los subtítulos y una altura de línea cómoda para los párrafos | `css/estilos.css` |
| 6 | **Página «Galería»** | Una página nueva, `galeria.html`, con la misma hoja de estilos, y un estilo propio para ella al final de la hoja | `galeria.html`, `css/estilos.css` |

> ⚠️ Las tareas 2, 3 y 4 agregan contenido **en el mismo lugar** de `index.html`, y las tareas 5 y 6 agregan reglas **al final** de `css/estilos.css`. Van a
> aparecer conflictos: resolverlos es parte del taller.

### 4️⃣ Ramas, integración y conflictos

- Cada tarea se hace en **una rama propia creada a partir de `master` actualizada**, con el nombre `tarea/<nombre-corto>` (por ejemplo, `tarea/proyectos`).
- Al terminar, cada persona **integra su rama en `master` en su repositorio local** y **publica `master`** en GitHub.
- Si GitHub rechaza la publicación porque otra persona publicó antes, hay que **traer los cambios del remoto**, integrarlos y volver a publicar.
- Cuando aparezca un **conflicto**, se resuelve **conservando el trabajo de las dos personas** (salvo que el grupo decida otra cosa) y se comprueba en el
  navegador que la página sigue viéndose bien antes de publicar.
- Al terminar, cada persona **borra su rama** (la local y, si la publicó, la del remoto).

## 📏 Reglas del equipo

- 🚫 **Nadie hace commits directamente en `master`**: todo cambio nace en una rama `tarea/…`.
- 🔄 **Antes de crear una rama y antes de integrarla**, actualizá tu `master` con lo que hay en GitHub.
- 🚫 **Nunca uses `git push --force`** sobre `master`: borraría el trabajo de otras personas.
- 💬 **Avisá al grupo** cuando publicás en `master`, para que el resto sepa que tiene que actualizarse.
- ✍️ Escribí **mensajes de commit claros**, que digan qué hiciste (por ejemplo, «Agrega la sección Contacto»).
- 🤝 Si un conflicto te afecta junto con otra persona, **resuélvanlo juntos**.
- 🧹 Una rama integrada **se borra**.

## 📦 Entregables

1. El repositorio del equipo en GitHub, con **todas las tareas integradas en `master`** y sin ramas `tarea/…` pendientes.
2. La salida de `git log --oneline --graph --all` (copiada como texto) de una persona del grupo, tomada al final.
3. Un breve **informe del equipo** (una página como máximo) con:
   - la tabla de tareas con el nombre de quien la hizo y el nombre de su rama;
   - los conflictos que aparecieron: en qué archivo, entre qué tareas, quién los resolvió y cómo;
   - una dificultad que tuvieron y cómo la solucionaron.

## 📋 Criterios de evaluación

- ✔️ Todas las personas del grupo figuran como colaboradoras y tienen **al menos un commit propio** en `master`.
- ✔️ Cada tarea se desarrolló en **su propia rama** con el nombre acordado.
- ✔️ El historial muestra **las integraciones** (commits de fusión) y no hay commits hechos directamente en `master` fuera de ellas.
- ✔️ Los conflictos se resolvieron **sin perder trabajo**: la página final incluye el aporte de todas las tareas y no quedan marcas de conflicto (`<<<<<<<`, `=======`, `>>>>>>>`).
- ✔️ La página final se ve correctamente en el navegador.
- ✔️ No quedan ramas `tarea/…` sin borrar, ni locales ni en GitHub.
- ✔️ El informe explica con claridad al menos un conflicto y su resolución.

## 🧩 Desafíos opcionales

- **Segunda ronda.** Cada persona toma la tarea de otra y la mejora (por ejemplo, agrega un enlace a la galería en el menú). Repitan el ciclo completo.
- **El mismo renglón.** Dos personas cambian, cada una en su rama, el **texto del título** `<h1>`. Integren las dos y decidan juntas qué título queda.
- **Rotación.** La persona propietaria invita a una persona de **otro** grupo, que clona el repositorio y aporta una tarea nueva.

## 🆘 Si se traban

La [Guía 02 — Solución del taller paso a paso](guia-02-solucion-taller-equipo.md) resuelve el taller paso a paso, con los comandos y lo que tenés que ver en
cada momento. Intentá primero con lo que ya sabés; usá la guía para destrabarte o, al final, para repasar.
