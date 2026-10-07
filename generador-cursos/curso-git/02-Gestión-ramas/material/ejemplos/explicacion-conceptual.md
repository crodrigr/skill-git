# Explicación conceptual — Gestión de Ramas

## Ramas

### Contexto

En la Clase 01 trabajaste siempre sobre una única línea de commits. Pero en un
proyecto real es común querer probar una idea, corregir un error puntual o
desarrollar una función nueva sin arriesgar el código que ya funciona.

### Concepto

Una **rama** (*branch*) es una línea de desarrollo paralela dentro del mismo
repositorio: permite hacer commits propios sin afectar la rama principal (por
convención llamada `master` o `main`) hasta que decidís incorporar esos cambios.

### Explicación

- `git branch` lista las ramas existentes y marca con `*` en cuál estás parado.
- `git branch <nombre>` crea una rama nueva, a partir del commit actual, sin moverte
  a ella.
- `git switch <nombre>` (o `git checkout <nombre>`) cambia a esa rama; los commits
  que hagas a partir de ahí quedan en esa rama, no en la anterior.
- `git switch -c <nombre>` crea la rama y cambia a ella en un solo paso.
- `git branch -d <nombre>` elimina una rama **ya fusionada** (eliminación segura:
  Git la rechaza si detecta trabajo sin fusionar). `git branch -D <nombre>` fuerza la
  eliminación igual, perdiendo esos commits si no están en otra rama.

Cada rama es, en esencia, un puntero a un commit; moverte entre ramas no duplica
archivos, solo cambia qué puntero estás siguiendo.

## Merge y conflictos

### Contexto

Una rama aislada es útil mientras trabajás, pero en algún momento esos cambios tienen
que volver a juntarse con el resto del proyecto.

### Concepto

**Merge** (fusión) es la operación que incorpora los commits de una rama en otra. Si
ambas ramas modificaron partes distintas, Git combina todo automáticamente. Si ambas
modificaron la **misma** parte de un archivo, Git no puede decidir solo: eso es un
**conflicto de fusión**, y requiere que la persona decida cómo queda el resultado
final.

### Explicación

- Para fusionar, primero te parás en la rama que va a **recibir** los cambios (por
  ejemplo, `main`) y ejecutás `git merge <rama-a-fusionar>`.
- Si no hay conflicto, Git crea automáticamente un commit de fusión (o avanza el
  puntero, según el caso) y listo.
- Si hay conflicto, Git marca el archivo afectado con marcadores especiales:

  ```text
  <<<<<<< HEAD
  contenido de la rama actual
  =======
  contenido de la rama que estás fusionando
  >>>>>>> nombre-de-la-rama
  ```

- Hay que editar el archivo a mano, dejando el contenido final correcto (eligiendo
  una versión, combinando ambas, o escribiendo algo nuevo) y **borrar** los
  marcadores `<<<<<<<`, `=======` y `>>>>>>>`.
- Después de resolver, se agrega el archivo al staging (`git add`) y se completa la
  fusión con `git commit` (Git ya deja preparado un mensaje por defecto).

## Repositorio remoto

### Contexto

Hasta ahora todo tu trabajo con Git vivió únicamente en tu computadora. Si se rompe el
disco, o si querés compartir el proyecto con alguien más, necesitás una copia en otro
lugar.

### Concepto

Un **repositorio remoto** es una copia de tu repositorio alojada en otro lugar —
normalmente en una plataforma como **GitHub** — a la que te conectás para enviar
(*push*) y traer (*pull*/*fetch*) cambios. Un mismo repositorio local puede estar
conectado a uno o más remotos; el más común se llama, por convención, `origin`.

### Explicación

**Crear el repositorio remoto (en GitHub)**:

1. Iniciar sesión en GitHub (crear una cuenta gratuita si todavía no tenés una).
2. Ir a "New repository", elegir un nombre, y **no** inicializarlo con un README si
   ya tenés un repositorio local (para evitar historiales que no coinciden).
3. GitHub te muestra una URL del nuevo repositorio (por ejemplo,
   `https://github.com/tu-usuario/tu-proyecto.git`).

**Conectar un repositorio local existente con el remoto**:

```bash
git remote add origin https://github.com/tu-usuario/tu-proyecto.git
git remote -v
```

- `git remote add <nombre> <url>` asocia una URL remota con un nombre (`origin` es el
  nombre convencional del remoto principal).
- `git remote -v` muestra los remotos configurados y sus URLs, para verificar que la
  conexión quedó bien hecha.

> 💡 Para que la validación de este material (y tus propias pruebas) no dependan de
> una cuenta real ni de internet, los ejemplos reproducibles de esta clase usan un
> repositorio **`--bare`** local como remoto. Un repositorio `--bare` es un
> repositorio de Git sin carpeta de trabajo (solo el historial): es exactamente lo
> que Git aloja del otro lado en GitHub. Para Git, conectarte a una ruta local
> `--bare` o a una URL de GitHub es mecánicamente lo mismo.

## Trabajo conjunto local/remoto

### Contexto

Tener el remoto conectado no sirve de mucho si el historial local y el remoto nunca
se sincronizan. Falta aprender a enviar tus cambios y a traer los que no tenés
todavía.

### Concepto

**Publicar** (`push`) envía tus commits locales al remoto. **Traer** (`pull` o
`fetch`) trae al local los commits que están en el remoto pero no en tu copia. Cuando
el remoto tiene commits que tu local no tiene, Git **rechaza** el `push` hasta que
primero traigas esos cambios: así evita que se pierda el trabajo de otra persona (o
el tuyo propio desde otra computadora).

### Explicación

- `git push origin master` (o `git push -u origin master` la primera vez, para que
  Git recuerde la relación entre tu rama local y la del remoto) publica tus commits.
- `git pull origin master` trae los commits nuevos del remoto y los combina con tu
  rama local (por dentro, hace un `fetch` seguido de un `merge`).
- Si el remoto tiene un commit que tu local no tiene, `git push` se rechaza con un
  mensaje como "Updates were rejected because the remote contains work that you do
  not have locally". La solución es `git pull` primero (para traer e integrar ese
  commit) y recién después volver a intentar `git push`.
- Este es el mismo mecanismo de "divergencia" que viste con las ramas locales
  (Explicación de Ramas): acá simplemente una de las dos "ramas" que divergieron es
  la del remoto.
