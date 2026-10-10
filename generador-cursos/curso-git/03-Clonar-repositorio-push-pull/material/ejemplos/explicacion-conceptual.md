# Explicación conceptual — Clonar un repositorio, hacer push y pull

Esta explicación asume que ya cursaste la Clase 01 (flujo local) y la Clase 02 (ramas,
merge, y creación y conexión de un repositorio remoto). No se repite ese contenido.

Resultados de aprendizaje de la clase:

- **RA-1**: clonar un proyecto desde un repositorio remoto.
- **RA-2**: descargar actualizaciones del remoto al repositorio local.
- **RA-3**: enviar cambios del repositorio local al remoto.
- **RA-4**: revertir cambios en el repositorio local y en el remoto.

---

## Clonar

_Resultados de aprendizaje: RA-1_

### Contexto

En la Clase 02 creaste un repositorio vacío en GitHub y lo conectaste con uno local que
ya existía. Pero lo más común en la práctica es lo contrario: el proyecto **ya existe**
en un remoto y necesitás una copia en tu máquina para trabajar sobre él.

### Concepto

**Clonar** es crear una copia local completa de un repositorio remoto. A diferencia de
copiar una carpeta, el clon trae:

- los archivos del proyecto,
- **todo el historial** de commits y todas las ramas del remoto,
- el remoto de origen **ya configurado** con el nombre `origin`.

| | Crear local + conectar (Clase 02) | Clonar |
|---|---|---|
| Punto de partida | Tenés un repositorio local | Existe un repositorio remoto |
| Conexión con el remoto | La hacés vos con `git remote add` | Ya viene hecha (`origin`) |
| Historial | Solo el tuyo | Todo el del remoto |

### Explicación

```bash
git clone <url>                 # crea la carpeta con el nombre del repositorio
git clone <url> mi-carpeta      # crea la carpeta con el nombre que elijas
```

Después de clonar, comprobá tres cosas: `git remote -v` (el origen está
configurado), `git log --oneline` (llegó el historial) y `git status` (la copia está
limpia y alineada con el remoto).

Si la carpeta de destino ya existe y no está vacía, Git se niega a clonar: elegí otro
nombre o ubicación.

**Preparación de la clase.** Antes de empezar, configurá cómo se integran los cambios
al hacer `git pull`, para que todos veamos el mismo comportamiento:

```bash
git config --global pull.rebase false     # integrar mediante fusión (merge)
git config --get pull.rebase              # debe imprimir: false
```

---

## Pull y push

_Resultados de aprendizaje: RA-2, RA-3_

### Contexto

Una vez que tenés una copia local, el trabajo ocurre en dos lugares a la vez: tu
repositorio y el remoto. Lo que hacés en uno no aparece en el otro hasta que lo
sincronizas.

### Concepto

Dos operaciones, una por dirección:

| Operación | Dirección | Qué hace |
|---|---|---|
| `git pull` | remoto → local | Trae los commits nuevos del remoto y los integra en tu rama |
| `git push` | local → remoto | Publica en el remoto los commits que este aún no tiene |

Dos reglas que lo explican casi todo:

1. **Los commits son locales hasta que hacés push.** Confirmar un cambio no lo
   comparte con nadie.
2. **Git solo acepta un push que avanza el historial remoto sin perder nada.** Si el
   remoto tiene commits que vos no tenés, el push se rechaza y debés traerlos primero.

### Explicación

**Pull.** Con novedades, Git descarga los commits y los integra. Si tu rama no tenía
commits propios, el avance es directo (*fast-forward*). Sin novedades, responde
`Already up to date.` Si tenés cambios **sin confirmar** en un archivo que el pull
también va a modificar, Git aborta para no pisarlos: confirmá tus cambios o guardalos
temporalmente con `git stash`, hacé el pull y recuperalos con `git stash pop`.

**Push.**

```bash
git push                              # envía la rama actual (si ya tiene seguimiento)
git push -u origin entrada-viaje      # primer push de una rama nueva
```

El `-u` enlaza tu rama local con la rama remota, de modo que después basta con
`git push` y `git pull`. Sin ese enlace, Git te indica el comando exacto a usar.

**Push rechazado.** Si otra persona publicó antes que vos, Git responde
`! [rejected] ... (fetch first)`. La solución es siempre la misma: `git pull`, resolver
un conflicto si aparece y volver a `git push`. Al hacer pull con commits locales que el
remoto no tiene (**historial divergente**), Git crea un commit de fusión y es posible
que abra el editor para su mensaje: guardá y cerrá.

---

## Ciclo de trabajo y autenticación

_Resultados de aprendizaje: RA-2, RA-3_

### Contexto

Con push y pull entendidos por separado, falta un hábito que evite los rechazos y los
conflictos grandes, y saber qué hacer cuando falla el acceso al remoto.

### Concepto

El ciclo recomendado al colaborar en una rama compartida es:

1. `git pull` — empezá actualizado.
2. Trabajá y confirmá (`git add`, `git commit`).
3. `git pull` — volvé a actualizarte antes de enviar.
4. `git push` — publica.

Cuando los cambios chocan en la misma parte de un archivo, el pull termina en un
**conflicto**: se resuelve igual que en la Clase 02 (editar los marcadores, `git add`,
`git commit`).

### Explicación

**Verificación del token.** En la Clase 02 creaste un *Personal Access Token* para
autenticarte por HTTPS. Antes de clonar o hacer push, comprobá que sigue vigente:

```bash
git ls-remote https://github.com/tu-usuario/diario.git
```

Si responde con una lista de referencias, el acceso funciona.

**Diagnóstico de errores de credenciales** _(no reproducible localmente: estos
mensajes los produce GitHub)_:

| Mensaje típico | Causa probable | Qué hacer |
|---|---|---|
| `fatal: Authentication failed for 'https://github.com/...'` | Token vencido, revocado o escrito mal | Generá un token nuevo y usalo como contraseña |
| `remote: Permission to usuario/repo.git denied to otro-usuario.` (error 403) | El token no tiene permisos de escritura sobre ese repositorio, o no sos colaborador | Revisá los permisos del token y de tu usuario en el repositorio |
| `remote: Repository not found.` | URL mal escrita, o repositorio privado sin acceso | Verificá la URL; si es privado, confirmá que tu cuenta tiene acceso |

Distinguir "URL mal escrita" de "sin permisos" evita perder tiempo: en el primer caso
el repositorio no existe para nadie; en el segundo, existe pero no para vos.

---

## Revertir cambios

_Resultados de aprendizaje: RA-4_

### Contexto

Tarde o temprano confirmarás —y quizá ya publicaste— un cambio equivocado. Hay que
corregirlo sin romper el trabajo de quienes ya descargaron el historial.

### Concepto

**Revertir** (`git revert`) crea un **commit nuevo** que deshace los cambios de un
commit anterior. El commit original **no se borra**: el historial solo crece.

Eso lo hace la opción segura en un remoto compartido: como no modifica commits ya
publicados, las demás personas reciben la corrección con un simple `git pull`.

| Quiero… | Herramienta | ¿Seguro en un remoto compartido? |
|---|---|---|
| Descartar cambios que aún no confirmé | `git restore <archivo>` | Sí (son solo locales) |
| Deshacer un commit **dejando constancia** en el historial | `git revert` | **Sí** |
| Borrar commits del historial (`reset`, o `push --force`) | `git reset` | **No** en ramas compartidas: reescribe historial publicado |

En esta clase solo se practica `git revert`; `restore` y `reset` se mencionan para que
sepas distinguirlos.

### Explicación

```bash
git log --oneline                   # localiza el commit erróneo y copia su identificador
git revert --no-edit <commit>       # crea el commit de reversión
git push                            # propaga la corrección al remoto
```

Las demás personas la reciben con `git pull`. Si la reversión no se envía, el remoto
sigue con el error: **revertir en local no corrige el remoto hasta que hacés push**.

**Conflicto de reversión.** Si commits posteriores modificaron las mismas líneas que
el commit que revertes, Git no puede deshacerlo solo: marca el conflicto. Se resuelve
editando el archivo, `git add` y `git revert --continue`. Si preferís desistir:
`git revert --abort`.

**Solo se menciona.** Revertir un commit de fusión (*merge*) exige indicar cuál de los
padres conservar (`git revert -m 1 <commit>`), y es posible revertir varios commits a
la vez. Ambos casos quedan fuera de la práctica de esta clase.
