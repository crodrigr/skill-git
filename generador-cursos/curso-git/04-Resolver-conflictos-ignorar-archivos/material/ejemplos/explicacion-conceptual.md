# 📚 Explicación conceptual — Resolver conflictos e ignorar archivos

Esta explicación asume que ya cursaste las Clases 01 a 03: flujo local, ramas y fusión,
un conflicto simple (Clase 02), y clonar, pull, push y revert (Clase 03). No se repite
ese contenido.

Resultados de aprendizaje de la clase:

- **RA-1**: resolver los conflictos que surgen al trabajar con repositorios locales y
  remotos.
- **RA-2**: evitar que ciertos archivos sean rastreados por Git, y corregir el caso de
  los que ya se rastrearon.

---

## ⚠️ Conflictos: causas e identificación

_Resultados de aprendizaje: RA-1_

### 🌍 Contexto

En la Clase 02 resolviste un conflicto de una sola línea entre dos ramas. En un proyecto
real los conflictos son más variados: varios archivos a la vez, varios bloques en un
mismo archivo, y también aparecen al hacer pull. Lo importante es leerlos con calma y
saber qué te está pidiendo Git.

### 💡 Concepto

Un **conflicto** aparece cuando Git no puede combinar automáticamente los cambios de dos
historias. Hay dos tipos frecuentes:

| Tipo | Cuándo ocurre | Cómo lo muestra Git |
|---|---|---|
| De contenido | Ambos lados modificaron las **mismas líneas** de un archivo | `both modified` (`UU`) y marcadores dentro del archivo |
| Modificado y eliminado | Un lado modificó un archivo y el otro lo **eliminó** | `deleted by them` (`UD`) o `deleted by us` (`DU`) |

Git fusiona solo todo lo que no choca: los archivos y las líneas que cambió un único
lado se integran sin pedirte nada. Un conflicto es la excepción, no la regla.

### 📖 Explicación

Los **marcadores** que Git escribe dentro del archivo ya los conocés:

```text
<<<<<<< HEAD
tu versión (la rama en la que estás parado)
=======
la versión entrante (la rama que fusionás o el remoto)
>>>>>>> otra-rama
```

Cuando una fusión se detiene por conflictos, **no la des por perdida**: el primer paso
es mirar el estado.

```bash
git status       # muestra los archivos en "Unmerged paths"
git status -s    # versión corta: UU = ambos modificaron, UD/DU = modificado y eliminado
```

En `Unmerged paths` están **solo** los archivos que requieren tu decisión. Los que
aparecen bajo `Changes to be committed` ya se fusionaron solos.

---

## ✍️ Resolver conflictos

_Resultados de aprendizaje: RA-1_

### 🌍 Contexto

Con los archivos en conflicto identificados, falta decidir qué contenido queda y dejar la
fusión completa, en local y, si corresponde, en el remoto.

### 💡 Concepto

Resolver es una decisión explícita sobre el contenido final, **bloque por bloque**. Para
cada bloque entre marcadores tenés tres opciones:

| Decisión | Qué dejás en el archivo |
|---|---|
| Lado propio | Solo la versión de arriba (`HEAD`) |
| Lado entrante | Solo la versión de abajo |
| Combinar | Un texto nuevo que integra lo mejor de ambos |

Un archivo puede tener varios bloques y cada uno admite una decisión distinta. Terminada
la edición, el archivo **no debe contener ningún marcador**.

### 📖 Explicación

El ciclo completo, para cualquier cantidad de archivos:

1. `git status` — identificá los archivos en conflicto.
2. Editá cada archivo: resolvé todos los bloques y borrá las tres líneas de marcadores
   de cada uno.
3. Comprobá que no quedó ninguno: `git diff --check` avisa con `leftover conflict marker`
   si todavía hay marcadores.
4. `git add <archivo>` — marca el archivo como resuelto.
5. `git commit` — completa la fusión.

Si un archivo quedó **modificado y eliminado**, la decisión es conservarlo (`git add`)
o eliminarlo (`git rm`); ambas lo marcan como resuelto.

**Conflicto al hacer pull.** `git pull` es un `fetch` seguido de una fusión, así que
puede terminar en conflicto exactamente igual. Se resuelve con el mismo ciclo y, una vez
completada la fusión, `git push` publica el resultado.

**Abortar.** Si preferís no continuar (por ejemplo, porque querés pensar el cambio antes
o consultar a tu compañera), `git merge --abort` vuelve al estado previo a la fusión sin
perder tus commits.

---

## 🧯 Otros conflictos y prevención

_Resultados de aprendizaje: RA-1_

### 🌍 Contexto

Las fusiones y los pulls no son las únicas operaciones que pueden terminar en conflicto.
Reconocer el patrón te permite resolverlos con la misma calma.

### 💡 Concepto

Cualquier operación que **aplica cambios sobre un estado distinto** puede chocar:

| Operación | Cuándo choca | Qué hacer |
|---|---|---|
| `git stash pop` | Los cambios guardados tocan líneas que cambiaron mientras tanto | Resolver; el stash **se conserva** hasta que lo descartes |
| `git revert` | Commits posteriores modificaron las mismas líneas | Resolver y `git revert --continue` |
| `git merge` / `git pull` | Ya visto | Resolver y confirmar |

### 📖 Explicación

**Con `stash pop`**, Git deja el conflicto en el archivo, avisa `The stash entry is kept
in case you need it again` y no borra el stash. Cuando termines de resolver (`git add`),
descartalo con `git stash drop`.

**Con `revert`**, el flujo es el de la Clase 03 más la resolución: editás, `git add` y
`git revert --continue`. Para desistir: `git revert --abort`.

**Hábitos que reducen los conflictos:**

- Traé los cambios con frecuencia (`git pull` antes de empezar y antes de enviar).
- Hacé commits pequeños y enfocados: un conflicto chico se resuelve rápido.
- Mantené las ramas de vida corta; cuanto más divergen, más chocan.
- Hablá con el equipo sobre quién modifica qué.

**Herramientas visuales.** Muchos editores de código muestran los conflictos con botones
para elegir un lado o combinar. Son una ayuda opcional: entender los marcadores te sirve
igual, y es lo que ves en cualquier terminal.

---

## 🙈 Ignorar archivos con .gitignore

_Resultados de aprendizaje: RA-2_

### 🌍 Contexto

No todo lo que hay en la carpeta de un proyecto debe versionarse: salidas generadas,
dependencias, configuración personal y, sobre todo, secretos. Versionar lo que no
corresponde ensucia el historial y puede exponer información sensible.

### 💡 Concepto

El archivo **`.gitignore`** contiene patrones que le indican a Git qué archivos **no
rastrear**. Se versiona como cualquier otro archivo, así que todo el equipo comparte las
mismas reglas.

| Se suele ignorar | Ejemplos |
|---|---|
| Archivos generados | `build/`, `*.log` |
| Dependencias | `dependencias/` |
| Configuración local | archivos propios de tu equipo |
| Secretos | claves, contraseñas, archivos de credenciales |

Un punto clave: **ignorar solo afecta a los archivos que Git todavía no rastrea.** Si un
archivo ya se había confirmado, agregarlo al `.gitignore` no lo deja de rastrear.

### 📖 Explicación

**Sintaxis de patrones** (una por línea):

| Patrón | Qué ignora |
|---|---|
| `credenciales.txt` | Ese nombre exacto, en cualquier carpeta |
| `*.log` | Todos los archivos que terminan en `.log` |
| `build/` | La carpeta `build` y todo lo que contiene |
| `/borrador.txt` | Solo `borrador.txt` en la raíz (la barra inicial ancla el patrón) |
| `!auditoria.log` | Excepción: **sí** se rastrea aunque otro patrón lo ignore |
| `# texto` | Comentario |

Las líneas en blanco se ignoran. El orden importa: una excepción (`!`) debe ir **después**
del patrón que anula.

**Dejar de rastrear un archivo ya rastreado.** Hay que quitarlo del índice **sin borrarlo
del disco**:

```bash
git rm --cached credenciales.txt    # deja de rastrearlo; el archivo sigue en tu carpeta
git commit -m "Dejar de rastrear credenciales"
```

**Saber por qué se ignora un archivo.** `git check-ignore -v <archivo>` imprime el
archivo de reglas, el número de línea y el patrón responsable. Si un archivo sigue
rastreado, no imprime nada: Git no lo considera ignorado.

**Excepciones.** Además de `!patrón`, `git add -f <archivo>` fuerza el rastreo de un
archivo ignorado. Usalo con criterio: suele ser señal de que el patrón es demasiado
amplio y conviene corregirlo con una negación.

**Exclusión local.** Si querés ignorar algo solo en tu máquina, sin compartirlo con el
equipo, agregá el patrón a `.git/info/exclude`. Funciona igual que `.gitignore` pero no se
versiona.

**Plantillas.** Para empezar, podés tomar un `.gitignore` por lenguaje o herramienta
desde el repositorio público de plantillas de GitHub y adaptarlo a tu proyecto. _(No
reproducible localmente: requiere internet.)_

**Si el secreto ya se publicó.** Dejar de rastrear un archivo **no borra el historial**:
las versiones anteriores siguen en los commits que ya están en el remoto. Si el archivo
contenía credenciales, considerá que quedaron expuestas y **reemplazalas** (generá una
nueva y revocá la anterior). Limpiar el historial queda fuera de esta clase.
