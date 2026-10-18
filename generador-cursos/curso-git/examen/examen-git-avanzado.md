# Examen Práctico de Git — Complejidad Alta

**Modalidad:** práctico, individual, con acceso a documentación oficial de Git.
**Duración sugerida:** 3 horas
**Entrega:** enlace al repositorio en **GitHub**, público o con acceso para el evaluador.
**Puntaje total:** 100 puntos (aprobación: 70)

## Objetivo

Demostrar dominio del flujo de trabajo con Git: creación de repositorio, manejo de ramas, integración a `master`, resolución de conflictos y sincronización con un remoto en GitHub (`merge`, `pull`, `push`).

## Escenario

Eres el desarrollador de un **sitio web** (HTML/CSS/JS simple) para la cafetería ficticia *"Café Aroma"*. El cliente irá solicitando cambios. Cada cambio debe desarrollarse de forma aislada en su propia rama y luego integrarse a `master`.

---

## Parte 1 — Creación del repositorio (10 pts)

1. Crea una carpeta `sitio-cafe-aroma` e inicializa un repositorio con `git init`.
2. Asegúrate de que la rama principal se llame **`master`**.
3. Crea la estructura mínima del sitio:
   ```
   sitio-cafe-aroma/
   ├── index.html
   ├── menu.html
   ├── contacto.html
   ├── css/styles.css
   ├── js/main.js
   ├── img/            (con al menos un archivo, p. ej. logo.png)
   ├── .gitignore
   └── README.md
   ```
4. El `.gitignore` debe excluir como mínimo: `node_modules/`, `*.log`, `.env`, `.DS_Store`, `dist/`.
5. Realiza el **commit inicial** con mensaje descriptivo.
6. Crea en **GitHub** un repositorio nuevo llamado `sitio-cafe-aroma` (en la web, o con `gh repo create sitio-cafe-aroma --public`). **No** lo inicialices con README, `.gitignore` ni licencia.
7. Agrégalo como remoto y sube `master`:
   ```bash
   git remote add origin https://github.com/<usuario>/sitio-cafe-aroma.git
   git push -u origin master
   ```
8. Verifica con `git remote -v` y comprobando en GitHub que el commit inicial aparece en `master`. Configura `master` como rama predeterminada del repositorio (*Settings → Branches*).
   > Autenticación: usa un *Personal Access Token* o SSH; no subas credenciales al repositorio.

## Parte 2 — Flujo de ramas por cambio (30 pts)

Se solicitan los siguientes cambios. **Cada uno va en su propia rama**, creada desde `master` actualizado, con **al menos 2 commits** por rama y mensajes claros.

| # | Rama | Cambio solicitado |
|---|------|-------------------|
| 1 | `feature/navbar` | Agregar barra de navegación común (Inicio, Menú, Contacto) en las tres páginas con estilos en `styles.css`. |
| 2 | `feature/menu-productos` | Poblar `menu.html` con al menos 6 productos (nombre, descripción, precio) y su estilo en `styles.css`. |
| 3 | `feature/formulario-contacto` | Agregar formulario en `contacto.html` (nombre, correo, mensaje) con validación básica en `main.js`. |
| 4 | `feature/footer` | Agregar pie de página con redes sociales y año en las tres páginas. |
| 5 | `fix/colores-contraste` | Corregir los colores de texto/fondo para mejorar el contraste. Modifica la **misma zona de `styles.css`** que `feature/navbar` (necesario para la Parte 3). |
| 6 | `hotfix/titulo-pagina` | Corregir el título (`<title>`) y encabezado `<h1>` de `index.html` por un error ortográfico. |

Requisitos:
- Los nombres de rama deben seguir exactamente la convención indicada.
- Los mensajes de commit deben seguir el formato `tipo: descripción` (`feat`, `fix`, `style`, `docs`).
- Al menos **dos ramas** deben subirse al remoto antes de integrarse (`git push -u origin <rama>`).

## Parte 3 — Integración a `master` y resolución de conflictos (30 pts)

1. Integra las ramas a `master` **en este orden**: `feature/navbar` → `feature/menu-productos` → `feature/formulario-contacto` → `feature/footer` → `fix/colores-contraste` → `hotfix/titulo-pagina`.
2. Se deben producir y resolver **al menos 3 conflictos reales**:
   - Un conflicto en `css/styles.css` (entre `feature/navbar` y `fix/colores-contraste`).
   - Un conflicto en `index.html` (entre `feature/navbar` y `hotfix/titulo-pagina`).
   - Un conflicto en `js/main.js` (entre `feature/formulario-contacto` y `feature/footer`). Ver Anexo.
3. Usa **ambas estrategias** al menos una vez cada una:
   - `git merge --no-ff` (merge commit visible en el historial).
   - Fast-forward (cuando sea posible) o `git rebase` de la rama sobre `master` antes de integrar.
4. Al resolver conflictos:
   - Conserva el trabajo relevante de ambas ramas (no basta con elegir un lado).
   - Elimina todos los marcadores `<<<<<<<`, `=======`, `>>>>>>>`.
   - El mensaje del merge debe indicar qué conflicto se resolvió y cómo.
5. Verifica que el sitio abre sin errores en el navegador tras cada integración.
6. Elimina las ramas ya integradas (local y remota) con `git branch -d` y `git push origin --delete`. **Conserva** `hotfix/titulo-pagina` sin eliminar como evidencia de uso.

## Parte 4 — Sincronización: pull, push y trabajo en dos copias (20 pts)

Simula el trabajo de dos desarrolladores sobre el mismo remoto:

1. Clona tu repositorio en otra carpeta (`sitio-cafe-aroma-dev2`) con `git clone`.
2. En la **copia 2**: crea un cambio en `README.md` (agrega sección "Instalación") directamente sobre `master`, haz commit y `git push`.
3. En la **copia 1** (original): sin hacer `pull`, modifica la **misma sección** del `README.md`, haz commit e intenta `git push`. Debe ser **rechazado** (non-fast-forward).
4. Resuelve usando `git pull` (merge) y, en una segunda situación similar, `git pull --rebase`. Resuelve el conflicto que aparezca y sube con `git push`.
5. Usa `git fetch` + `git log origin/master..master` (o similar) al menos una vez para inspeccionar diferencias antes de integrar.
6. Crea y sube un **tag** `v1.0.0` sobre el estado final de `master` (`git tag -a` y `git push origin v1.0.0`).

## Parte 5 — Revertir y cierre del repositorio (10 pts)

Todo se demuestra con comandos y resultados en el repositorio; **no hay preguntas teóricas**.

1. Crea la rama `feature/banner-promo` con un banner en `index.html` (`<div class="promo">¡2x1 en cappuccino!</div>`) y este estilo al final de `styles.css`: `.promo { background: #ffcc80; color: #3e2723; text-align: center; padding: .8rem; font-weight: bold; }`. Intégrala a `master` con `git merge --no-ff` y haz `push`.
2. Deshaz ese merge **sin reescribir el historial** usando `git revert -m 1 <hash-del-merge>` y haz `push`. El banner debe desaparecer del sitio y el merge original debe seguir en el historial.
3. Crea el archivo `EVIDENCIA.md` con la salida de `git log --oneline --graph --all --decorate` en un bloque de código, haz commit y `push` a `master`.
4. Confirma que `git status` queda limpio y que `master` local y `origin/master` apuntan al mismo commit.


## Anexo — Código que debe escribir el estudiante

> Copia el código tal cual. Está diseñado para que los conflictos de la Parte 3 aparezcan de forma natural: **respeta las líneas que cada rama modifica** y el orden de integración indicado.

### A. Código base (commit inicial, rama `master`)

**`index.html`** (`menu.html` y `contacto.html` son iguales cambiando `<title>`, el contenido de `<main>` y el marcador indicado)

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cafe Aroma - Inicio</title>
  <link rel="stylesheet" href="css/styles.css">
</head>
<body>
  <header>
    <h1>Cafe Aroma</h1>
  </header>

  <main>
    <h2>Bienvenido</h2>
    <p>El mejor café de especialidad, tostado cada mañana.</p>
  </main>

  <!-- FOOTER -->
  <script src="js/main.js"></script>
</body>
</html>
```

- `menu.html`: `<title>Cafe Aroma - Menú</title>`, `<h2>Nuestro menú</h2>` y, dentro de `<main>`, la línea `<!-- PRODUCTOS -->`.
- `contacto.html`: `<title>Cafe Aroma - Contacto</title>`, `<h2>Contacto</h2>` y, dentro de `<main>`, la línea `<!-- FORMULARIO -->`.
- Las tres páginas mantienen `<header><h1>Cafe Aroma</h1></header>` idéntico y la línea `<!-- FOOTER -->`.

**`css/styles.css`**

```css
/* ===== BASE ===== */
* { box-sizing: border-box; }
body { font-family: Arial, sans-serif; margin: 0; background: #f5f0e6; color: #333; }
main { max-width: 900px; margin: 2rem auto; padding: 0 1rem; }

/* ===== CABECERA ===== */
header {
  background: #6f4e37;
  color: #cccccc;
  padding: 1rem;
}
header h1 { margin: 0; }

/* ===== MENU ===== */

/* ===== CONTACTO ===== */

/* ===== FOOTER ===== */
```

**`js/main.js`**

```js
console.log("Cafe Aroma cargado");
```

---

### B. Código por rama

#### 1. `feature/navbar`

En **las tres páginas** reemplaza el bloque `<header>` por:

```html
  <header>
    <h1 class="logo">Cafe Aroma</h1>
    <nav>
      <a href="index.html">Inicio</a>
      <a href="menu.html">Menú</a>
      <a href="contacto.html">Contacto</a>
    </nav>
  </header>
```

En `styles.css` reemplaza el bloque `/* ===== CABECERA ===== */` por:

```css
/* ===== CABECERA ===== */
header {
  background: #4e342e;
  color: #cccccc;
  padding: 1rem 2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
header h1 { margin: 0; }
nav a { color: #f5f0e6; text-decoration: none; margin-left: 1.5rem; }
nav a:hover { text-decoration: underline; }
```

#### 2. `feature/menu-productos`

En `menu.html` reemplaza `<!-- PRODUCTOS -->` por:

```html
    <ul class="productos">
      <li><h3>Espresso</h3><p>Shot intenso de café de origen único.</p><span class="precio">$2.50</span></li>
      <li><h3>Cappuccino</h3><p>Espresso, leche vaporizada y espuma.</p><span class="precio">$3.50</span></li>
      <li><h3>Latte</h3><p>Suave, con abundante leche cremosa.</p><span class="precio">$3.80</span></li>
      <li><h3>Mocca</h3><p>Café, chocolate y crema batida.</p><span class="precio">$4.20</span></li>
      <li><h3>Cold Brew</h3><p>Infusionado en frío durante 18 horas.</p><span class="precio">$4.00</span></li>
      <li><h3>Croissant</h3><p>Hojaldre de mantequilla recién horneado.</p><span class="precio">$2.90</span></li>
    </ul>
```

En `styles.css`, debajo del comentario `/* ===== MENU ===== */`:

```css
.productos { list-style: none; padding: 0; display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 1rem; }
.productos li { background: #fff; padding: 1rem; border-radius: 8px; box-shadow: 0 2px 4px rgba(0,0,0,.1); }
.productos h3 { margin-top: 0; color: #4e342e; }
.precio { font-weight: bold; color: #8b5a2b; }
```

#### 3. `feature/formulario-contacto`

En `contacto.html` reemplaza `<!-- FORMULARIO -->` por:

```html
    <form id="form-contacto" novalidate>
      <label for="nombre">Nombre</label>
      <input type="text" id="nombre" name="nombre" required>

      <label for="correo">Correo</label>
      <input type="email" id="correo" name="correo" required>

      <label for="mensaje">Mensaje</label>
      <textarea id="mensaje" name="mensaje" rows="5" required></textarea>

      <button type="submit">Enviar</button>
      <p id="estado" role="alert"></p>
    </form>
```

En `styles.css`, debajo de `/* ===== CONTACTO ===== */`:

```css
form { display: grid; gap: .5rem; max-width: 500px; }
input, textarea { padding: .6rem; border: 1px solid #bbb; border-radius: 4px; font: inherit; }
button { padding: .7rem; background: #4e342e; color: #fff; border: 0; border-radius: 4px; cursor: pointer; }
#estado { color: #b00020; min-height: 1.2em; }
```

En `js/main.js` **reemplaza** la línea `console.log(...)` por:

```js
const form = document.getElementById("form-contacto");
if (form) {
  form.addEventListener("submit", (e) => {
    e.preventDefault();
    const estado = document.getElementById("estado");
    const nombre = form.nombre.value.trim();
    const correo = form.correo.value.trim();
    const mensaje = form.mensaje.value.trim();
    const correoValido = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(correo);

    if (!nombre || !correoValido || mensaje.length < 10) {
      estado.textContent = "Completa todos los campos (mensaje de al menos 10 caracteres) con un correo válido.";
      return;
    }
    estado.style.color = "green";
    estado.textContent = "¡Gracias " + nombre + ", te responderemos pronto!";
    form.reset();
  });
}
```

#### 4. `feature/footer`

En **las tres páginas** reemplaza `<!-- FOOTER -->` por:

```html
  <footer>
    <p>&copy; <span id="anio"></span> Cafe Aroma · Calle Principal 123</p>
    <p>
      <a href="https://facebook.com">Facebook</a> ·
      <a href="https://instagram.com">Instagram</a> ·
      <a href="https://x.com">X</a>
    </p>
  </footer>
```

En `styles.css`, debajo de `/* ===== FOOTER ===== */`:

```css
footer { background: #3e2723; color: #f5f0e6; text-align: center; padding: 1.5rem 1rem; margin-top: 3rem; }
footer a { color: #ffcc80; }
```

En `js/main.js` **reemplaza** la línea `console.log(...)` por:

```js
document.getElementById("anio").textContent = new Date().getFullYear();
```

#### 5. `fix/colores-contraste`

En `styles.css` reemplaza **solo** el bloque `header { ... }` de la cabecera (sin tocar `header h1`) por:

```css
header {
  background: #3e2723;
  color: #ffffff;
  padding: 1rem;
}
```

Y en `body` cambia `color: #333;` por `color: #212121;`.

#### 6. `hotfix/titulo-pagina`

En `index.html` corrige el error ortográfico (falta la tilde):

```html
  <title>Café Aroma - Inicio</title>
...
    <h1>Café Aroma</h1>
```

---

### C. Conflictos que deben aparecer (guía para el evaluador)

| Merge | Archivo | Causa |
|-------|---------|-------|
| `fix/colores-contraste` → `master` | `css/styles.css` | `feature/navbar` y el fix modifican las mismas líneas de `header { ... }` (`background`, `padding`, `color`). Resolver combinando `display:flex` de navbar con los colores del fix. |
| `hotfix/titulo-pagina` → `master` | `index.html` | `feature/navbar` cambió `<h1>Cafe Aroma</h1>` a `<h1 class="logo">…</h1>`; el hotfix corrige la misma línea. Resolver dejando `<h1 class="logo">Café Aroma</h1>`. |
| `feature/footer` → `master` | `js/main.js` | `feature/formulario-contacto` y `feature/footer` reemplazan la misma línea `console.log`. Resolver conservando **ambos** bloques de código. |

> Si el estudiante integra en otro orden, los conflictos pueden aparecer en otros merges; lo importante es que resuelva al menos tres sin perder el trabajo de ninguna rama.

---

---

## Entrega

Envía **un único enlace** al repositorio en GitHub (debe abrirse sin pedir permisos al evaluador), por ejemplo:

```
https://github.com/<usuario>/sitio-cafe-aroma
```

El repositorio debe contener, en el remoto:
- La rama `master` con todos los cambios integrados.
- El historial completo (no se acepta historial aplastado ni reiniciado).
- El tag `v1.0.0`.
- La rama `hotfix/titulo-pagina`.
- El archivo `EVIDENCIA.md` y el commit de `revert` del banner.

## Rúbrica de evaluación

| Criterio | Puntos | Indicadores |
|----------|:------:|-------------|
| Repositorio y estructura inicial | 10 | `master`, estructura, `.gitignore` correcto, commit inicial, repositorio creado en GitHub, `origin` configurado y `master` como rama predeterminada |
| Ramas por cambio y commits | 30 | 6 ramas con nombres correctos, ≥2 commits por rama, mensajes `tipo: descripción`, ramas subidas al remoto |
| Integración y conflictos | 30 | Orden correcto, ≥3 conflictos resueltos sin perder trabajo, uso de merge `--no-ff` y rebase/ff, sitio funcional, limpieza de ramas |
| Sincronización (pull/push/fetch/tag) | 20 | Push rechazado y resuelto, `pull` y `pull --rebase`, `fetch`, tag `v1.0.0` |
| Revertir y cierre | 10 | Merge de `feature/banner-promo`, `revert -m 1` subido sin reescribir historial, `EVIDENCIA.md` con el grafo, `master` = `origin/master` |
| **Total** | **100** | |

**Penalizaciones:**
- −10: marcadores de conflicto olvidados en algún archivo.
- −10: uso de `push --force` sobre `master` o historial reescrito en el remoto.
- −5: commits sin mensaje descriptivo (p. ej. "update", "cambios").
- −5: archivos ignorables subidos al repositorio.

## Verificación sugerida para el evaluador

```bash
git clone <url> && cd sitio-cafe-aroma
git branch -a                                  # ramas esperadas
git log --oneline --graph --all --decorate     # merges y ramas
git log --merges --oneline                     # ≥ 3 merge commits
git tag                                        # v1.0.0
grep -rn "<<<<<<<\|>>>>>>>" . --exclude-dir=.git   # no debe haber resultados
git ls-files | grep -E "node_modules|\.log$|\.env$"  # no debe haber resultados
```
