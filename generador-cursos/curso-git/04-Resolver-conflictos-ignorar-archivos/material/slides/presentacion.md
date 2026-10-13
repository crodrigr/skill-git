# Resolver conflictos e ignorar archivos

---

## Objetivos de la clase

- Resolver conflictos en varios archivos y con repositorio remoto.
- Abortar una fusión y reconocer otros conflictos.
- Controlar qué archivos se rastrean con `.gitignore`.

> Prerrequisitos: Clases 01 a 03.

---

## ¿Por qué surgen los conflictos?

- Dos historias cambiaron **las mismas líneas** de un archivo.
- O un lado modificó un archivo que el otro eliminó.
- Todo lo demás Git lo fusiona solo.

---

## Identificar los conflictos

```bash
git status
git status -s     # UU = ambos modificaron
```

Solo los archivos en `Unmerged paths` necesitan tu decisión.

---

## Resolver bloques y archivos

```text
<<<<<<< HEAD
tu versión
=======
la entrante
>>>>>>> otra-rama
```

Por bloque: lado propio, lado entrante o combinar.
Sin marcadores al terminar: `git diff --check`.

---

## Completar la fusión

```bash
git add archivo
git commit
```

Un `git add` por cada archivo resuelto, y un solo commit.

---

## Conflicto con remoto

```bash
git pull        # se detiene en conflicto
# resolver, git add, git commit
git push
```

`git pull` es traer y fusionar: puede chocar igual.

---

## Abortar

```bash
git merge --abort
```

Vuelve al estado previo al pull. No perdés tus commits.

---

## Otros conflictos y prevención

| Operación | Cómo seguir |
|---|---|
| `git stash pop` | Resolver; `git stash drop` |
| `git revert` | Resolver; `git revert --continue` |

Traé seguido, commits chicos, ramas cortas, hablá con el equipo.

---

## ¿Qué es .gitignore?

- Archivo versionado con patrones de lo que Git **no** rastrea.
- Se ignoran: generados, dependencias, config local, secretos.
- Solo afecta a archivos **todavía no rastreados**.

---

## Patrones

```text
build/            # carpeta
*.log             # comodín
!auditoria.log    # excepción
/borrador.txt     # solo en la raíz
```

---

## Dejar de rastrear

```bash
git rm --cached credenciales.txt
git commit -m "Dejar de rastrear credenciales"
git check-ignore -v credenciales.txt
```

`--cached` conserva el archivo en tu carpeta.

---

## Archivos sensibles

- Dejar de rastrear **no limpia el historial**.
- Una credencial publicada se considera expuesta.
- Reemplazala: generá una nueva y revocá la anterior.

---

## Actividad práctica

Taller 01: tu `diario` con dos copias.

- Crear un `.gitignore` y compartirlo.
- Provocar, resolver y abortar un conflicto.
- Dejar de rastrear un archivo.

---

## Resumen

- Conflicto = decisión humana: propio, entrante o combinar.
- `status`, editar, `diff --check`, `add`, `commit`.
- `.gitignore` evita; `rm --cached` corrige.

---

## Evaluación

Quiz 01: 8 ítems sobre conflictos y `.gitignore`.
