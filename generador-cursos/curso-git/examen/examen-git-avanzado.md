# Examen Práctico de Git — Complejidad Alta

**Modalidad:** práctico, individual, con acceso a documentación oficial de Git.
**Duración sugerida:** 3 horas
**Entrega:** enlace al repositorio remoto (GitHub/GitLab/Bitbucket), público o con acceso para el evaluador.
**Puntaje total:** 100 puntos (aprobación: 70)

## Objetivo

Demostrar dominio del flujo de trabajo con Git: creación de repositorio, manejo de ramas, integración a `master`, resolución de conflictos y sincronización con un remoto (`merge`, `pull`, `push`).

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
6. Crea un repositorio vacío en el servicio remoto, agrégalo como `origin` y sube `master` con `git push -u origin master`.

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
   - Un conflicto en `index.html` o en el pie/encabezado común (entre `feature/footer` y `feature/navbar` u otra rama).
   - Un conflicto adicional a elección (p. ej. `README.md` o `js/main.js`).
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

## Parte 5 — Documentación y evidencia (10 pts)

En `README.md` del repositorio incluye una sección **"Informe del examen"** con:

1. Salida de `git log --oneline --graph --all --decorate` (pegada como bloque de código) que muestre el historial con ramas y merges.
2. Una tabla con los conflictos resueltos: archivo, ramas involucradas, cómo se resolvió.
3. Respuestas breves (2–4 líneas cada una):
   - ¿Cuál es la diferencia entre `merge` y `rebase`, y cuándo usarías cada uno?
   - ¿Qué diferencia hay entre `git fetch` y `git pull`?
   - ¿Por qué fue rechazado el `push` en la Parte 4 y qué indica Git?
   - ¿Qué hace `git push --force` y por qué es peligroso en `master`?
   - ¿Cómo deshacerías un merge ya subido al remoto sin reescribir el historial? (menciona `git revert -m`).

---

## Entrega

Envía **un único enlace** al repositorio remoto, por ejemplo:

```
https://github.com/<usuario>/sitio-cafe-aroma
```

El repositorio debe contener, en el remoto:
- La rama `master` con todos los cambios integrados.
- El historial completo (no se acepta historial aplastado ni reiniciado).
- El tag `v1.0.0`.
- La rama `hotfix/titulo-pagina`.
- El README con el informe.

## Rúbrica de evaluación

| Criterio | Puntos | Indicadores |
|----------|:------:|-------------|
| Repositorio y estructura inicial | 10 | `master`, estructura, `.gitignore` correcto, commit inicial, remoto configurado |
| Ramas por cambio y commits | 30 | 6 ramas con nombres correctos, ≥2 commits por rama, mensajes `tipo: descripción`, ramas subidas al remoto |
| Integración y conflictos | 30 | Orden correcto, ≥3 conflictos resueltos sin perder trabajo, uso de merge `--no-ff` y rebase/ff, sitio funcional, limpieza de ramas |
| Sincronización (pull/push/fetch/tag) | 20 | Push rechazado y resuelto, `pull` y `pull --rebase`, `fetch`, tag `v1.0.0` |
| Documentación e informe | 10 | Grafo del log, tabla de conflictos, respuestas conceptuales correctas |
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
