# Explicación conceptual — Fundamentos Git

## Control de versiones

### Contexto

Imagina que estás escribiendo un documento importante y vas guardando copias con
nombres como `informe-final.docx`, `informe-final-v2.docx`, `informe-final-v2-ahora-si.docx`.
Al tercer o cuarto cambio ya no sabes cuál es la versión buena, qué cambiaste entre una
y otra, ni cómo volver atrás si algo salió mal. Con código pasa lo mismo, pero además
varias personas pueden estar modificando los mismos archivos al mismo tiempo.

### Concepto

El **control de versiones** es la práctica de registrar los cambios de un conjunto de
archivos a lo largo del tiempo, de forma que se pueda consultar el historial completo,
volver a una versión anterior, y entender exactamente qué cambió, cuándo y por qué.
**Git** es el sistema de control de versiones más usado hoy para administrar código
fuente (aunque sirve para cualquier archivo de texto).

### Explicación

Git resuelve tres problemas a la vez:

- **Historial**: cada cambio confirmado queda guardado para siempre, con una
  descripción de qué se hizo.
- **Reversión**: si algo se rompe, se puede volver a un estado anterior conocido.
- **Colaboración**: varias personas pueden trabajar sobre el mismo proyecto sin
  pisarse el trabajo (esto se profundiza en un módulo posterior del curso, con
  repositorios remotos y ramas).

A diferencia de guardar copias manuales de archivos, Git no duplica todo el proyecto en
cada cambio: guarda únicamente las diferencias necesarias para reconstruir cualquier
versión del historial, de forma eficiente.

## Instalación y configuración

### Contexto

Antes de poder usar cualquiera de estas ventajas, Git tiene que estar instalado en la
computadora y "saber quién sos": cada cambio que se confirme queda firmado con un
nombre y un correo electrónico.

### Concepto

La instalación de Git depende del sistema operativo (Windows, macOS o Linux), pero el
resultado es el mismo en los tres casos: un comando `git` disponible en la terminal. La
configuración inicial establece la **identidad** que acompañará a cada commit que se
haga desde esa computadora.

### Explicación

**Instalación**:

- **Windows**: se descarga e instala "Git for Windows" desde el sitio oficial, que
  además agrega una terminal (Git Bash).
- **macOS**: suele venir preinstalado junto con las Herramientas de Línea de Comandos
  de Xcode; si no, se instala con el gestor de paquetes `brew install git` o desde el
  instalador oficial.
- **Linux**: se instala con el gestor de paquetes de la distribución, por ejemplo
  `sudo apt install git` (Debian/Ubuntu) o `sudo dnf install git` (Fedora).

En los tres casos, la instalación se verifica con:

```bash
git --version
```

**Configuración de identidad** (una sola vez por computadora):

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu-correo@ejemplo.com"
```

Para verificar que quedó guardada:

```bash
git config --list
```

Si se intenta confirmar un commit sin esta configuración, Git se detiene y pide
configurarla antes de continuar (ver Ejercicio Básico 01).

## Repositorio y staging

### Contexto

Con Git ya instalado y configurado, el siguiente paso es convertir una carpeta
cualquiera en un proyecto versionado. Pero no todo lo que se modifica en esa carpeta
queda registrado automáticamente: hace falta decirle a Git, explícitamente, qué
cambios se quieren confirmar.

### Concepto

Un **repositorio** es una carpeta bajo control de Git: a partir de `git init`, Git
empieza a llevar un registro de los archivos que elijamos. El **área de staging**
(*staging area*, también llamada índice) es un paso intermedio entre "modificar un
archivo" y "confirmarlo en el historial": ahí se preparan los cambios que van a formar
parte del próximo commit.

### Explicación

- `git init` crea un repositorio nuevo en la carpeta actual (una subcarpeta oculta
  `.git/` guarda todo el historial).
- `git status` muestra el estado del repositorio: qué archivos están modificados,
  cuáles están en el área de staging y cuáles todavía no se están rastreando.
- `git add <archivo>` mueve los cambios de un archivo al área de staging. También se
  puede usar `git add .` para agregar todos los cambios pendientes de la carpeta
  actual.

¿Por qué un paso intermedio? Porque permite armar un commit con **exactamente** los
cambios que se quieren confirmar, incluso si hay otros archivos modificados que todavía
no están listos para confirmarse.

## Commits e historial

### Contexto

Ya con cambios en el área de staging, falta el paso que realmente los registra en el
historial del proyecto: el commit.

### Concepto

Un **commit** es una fotografía confirmada del estado del proyecto en un momento dado,
acompañada de un mensaje que describe qué cambió y por qué. El conjunto de todos los
commits de un repositorio forma su **historial**, que se puede consultar y navegar sin
modificar nada.

### Explicación

- `git commit -m "mensaje descriptivo"` confirma los cambios que están en el área de
  staging y los agrega como un nuevo commit al historial.
- Un buen mensaje de commit describe **qué** cambió y, si hace falta, **por qué** (por
  ejemplo: `"Agregar validacion de stock antes de confirmar la venta"`, no simplemente
  `"cambios"` o `"update"`).
- `git log` muestra el historial de commits, del más reciente al más antiguo, con su
  identificador, autor, fecha y mensaje.
- `git show <commit>` muestra el contenido completo de un commit puntual: qué cambió
  exactamente en ese commit.

Navegar el historial con `git log` y `git show` es una operación de **solo lectura**:
no modifica el repositorio, a diferencia de otros comandos de Git que sí lo hacen (y
que quedan fuera del alcance de esta clase).

## README.md

### Contexto

Un repositorio bien gestionado, con commits claros, igual puede ser difícil de
entender para alguien que lo abre por primera vez si no explica de qué se trata el
proyecto.

### Concepto

El archivo **`README.md`** es el primer documento que alguien lee al llegar a un
repositorio (muchas plataformas, incluido GitHub, lo muestran automáticamente en la
página principal del proyecto). Se escribe en Markdown y describe, como mínimo, qué es
el proyecto y cómo se usa.

### Explicación

Un `README.md` mínimo razonable suele incluir:

- Un título (el nombre del proyecto).
- Una descripción breve: qué hace o para qué sirve.
- Cómo usarlo o ejecutarlo, si aplica.

Como es un archivo más del repositorio, se crea y se versiona igual que cualquier
otro: se escribe su contenido, se agrega al área de staging con `git add README.md`, y
se confirma con `git commit`, con un mensaje tan descriptivo como el de cualquier otro
cambio (por ejemplo, `"Agregar README con descripcion del proyecto"`).
