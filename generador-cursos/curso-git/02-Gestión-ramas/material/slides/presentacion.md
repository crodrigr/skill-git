# Gestión de Ramas

---

## Objetivos de la clase

- Entender qué es una rama y para qué se usa.
- Crear, cambiar y eliminar ramas correctamente.
- Fusionar ramas y resolver un conflicto simple.
- Crear un repositorio remoto y trabajar en conjunto local/remoto.

> Prerrequisito: Clase 01 — Fundamentos Git (repositorio local, staging, commits).

---

## ¿Qué es una rama?

- Línea de desarrollo paralela dentro del mismo repositorio.
- Aísla cambios sin afectar la rama principal (`master`/`main`).
- Técnicamente: un puntero a un commit.

---

## Crear, cambiar y eliminar ramas

```bash
git branch                 # listar ramas
git switch -c mi-rama      # crear y cambiar en un paso
git branch -d mi-rama       # eliminar (segura: rechaza si falta fusionar)
git branch -D mi-rama       # eliminar forzada
```

---

## Merge y conflictos

```bash
git merge mi-rama -m "Mensaje de la fusion"
```

- Sin conflicto: Git combina todo solo (fast-forward o commit de fusión).
- Con conflicto: marcadores `<<<<<<<` / `=======` / `>>>>>>>` → resolver a mano,
  `git add`, `git commit`.

---

## ¿Qué es un repositorio remoto?

- Copia de tu repositorio alojada en otro lugar (GitHub).
- Permite respaldar y compartir el historial del proyecto.
- El remoto principal se llama, por convención, `origin`.

---

## Conexión local-remoto

```bash
git remote add origin https://github.com/usuario/proyecto.git
git remote -v
```

> En esta clase, para practicar sin cuenta ni internet, usamos un repositorio
> `--bare` local como remoto: a Git le da igual, el comportamiento es el mismo.

---

## Publicar y traer cambios

```bash
git push -u origin master   # publicar (primera vez)
git pull origin master       # traer y combinar
```

- Si el remoto tiene un commit que no tenés: `push` rechazado → `pull` primero.

---

## Flujo de trabajo conjunto

1. Antes de empezar a trabajar: `git pull`.
2. Trabajar en una rama, commitear seguido.
3. Fusionar a `master` y `git push`.
4. Si el `push` se rechaza: `git pull`, resolver conflicto si hay, reintentar.

---

## Actividad práctica

**Taller 01**: en tu propio proyecto —

1. Crear una rama, trabajar, fusionarla y eliminarla.
2. Crear un repositorio remoto en GitHub y conectarlo.
3. Publicar tu historial.
4. Traer un cambio hecho "del otro lado".

---

## Resumen

- Rama: línea de desarrollo paralela; `branch`/`switch -c`/`merge`/`branch -d`.
- Conflicto: marcadores `<<<<<<<`/`=======`/`>>>>>>>`, resolución manual.
- Remoto: copia del repositorio en GitHub; `remote add`, `push`, `pull`.
- Push rechazado por divergencia → `pull` primero, resolver si hay conflicto.
- Lo que sigue (módulo posterior): *pull requests* y colaboración en equipo.

---

## Evaluación

Quiz de 8 preguntas sobre ramas, merge, conflictos y repositorios remotos.
