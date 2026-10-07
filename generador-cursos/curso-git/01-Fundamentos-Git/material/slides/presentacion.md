# Fundamentos Git

---

## Objetivos de la clase

- Entender qué es el control de versiones y por qué importa.
- Instalar y configurar Git.
- Crear y gestionar un repositorio local: staging, commits, historial.
- Documentar un proyecto con `README.md`.

---

## ¿Qué es el control de versiones?

- Registra los cambios de un proyecto a lo largo del tiempo.
- Permite volver a una versión anterior.
- Permite colaborar sin pisarse el trabajo.

> Sin él: copias manuales (`final-v2-ahora-si.docx`) y cambios perdidos.

---

## Instalación y configuración de Git

```bash
git --version                              # verificar instalación
git config --global user.name "Tu Nombre"
git config --global user.email "tu@correo.com"
```

- Windows: Git for Windows · macOS: `brew install git` · Linux: `apt`/`dnf install git`
- La identidad configurada firma cada commit.

---

## Repositorios locales

```bash
git init          # crea el repositorio (.git/)
git status        # muestra el estado actual
```

- Una carpeta + `.git/` = un proyecto versionado.

---

## El área de staging

```bash
git add README.md   # prepara un cambio puntual
git add .            # prepara todos los cambios
```

- Paso intermedio entre "modificar" y "confirmar".
- Permite elegir exactamente qué entra en el próximo commit.

---

## Commits

```bash
git commit -m "Agregar README inicial"
```

- Una fotografía confirmada del proyecto, con mensaje descriptivo.
- Mensajes claros: qué cambió y, si hace falta, por qué.

---

## Navegación del historial

```bash
git log              # historial completo
git show <commit>    # detalle de un commit puntual
git show HEAD~1       # "un commit antes del mas reciente"
```

- Solo lectura: no modifica el repositorio.

---

## El archivo README.md

- Primer documento que alguien lee al abrir el repositorio.
- Mínimo: título, descripción breve, cómo usarlo.
- Se versiona igual que cualquier otro archivo: `git add` + `git commit`.

---

## Actividad práctica

**Taller 01**: desde cero, en tu propia computadora —

1. Repositorio nuevo (`git init`).
2. Al menos 3 commits con mensajes descriptivos.
3. Un `README.md` propio, confirmado con su commit.
4. Navegar el historial e identificar un commit puntual.

---

## Resumen

- Control de versiones: historial, reversión, colaboración.
- Git: instalar → configurar identidad → `init` → `add` → `commit` → `log`/`show`.
- `README.md`: documenta el proyecto, se versiona como cualquier archivo.
- Lo que sigue (módulo posterior): repositorios remotos y GitHub.

---

## Evaluación

Quiz de 8 preguntas sobre los conceptos y comandos de esta clase.
