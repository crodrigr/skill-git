# Clonar un repositorio, hacer push y pull

---

## Objetivos de la clase

- Clonar cualquier proyecto desde un repositorio remoto.
- Descargar y enviar cambios con `git pull` y `git push`.
- Revertir cambios en local y en remoto con `git revert`.

> Prerrequisitos: Clase 01 (flujo local) y Clase 02 (ramas y remoto).

---

## Preparación

```bash
git config --global pull.rebase false
git ls-remote https://github.com/tu-usuario/diario.git
```

- Primera línea: todos integramos con fusión.
- Segunda línea: comprobá que tu token sigue vigente.

---

## ¿Qué es clonar?

- Copia local **completa** de un repositorio remoto.
- Trae archivos, historial y ramas.
- `origin` ya viene configurado.

---

## Clonar y verificar

```bash
git clone <url> [carpeta]
git remote -v
git log --oneline
```

Si la carpeta existe y no está vacía, Git se niega.

---

## Local y remoto: dos lugares

| Comando | Dirección |
|---|---|
| `git pull` | remoto → local |
| `git push` | local → remoto |

Un commit es local hasta que hacés `push`.

---

## Pull: descargar cambios

```bash
git pull
```

- Con novedades: trae e integra los commits.
- Sin novedades: `Already up to date.`
- Cambios sin confirmar en conflicto: `git stash`.

---

## Push: enviar cambios

```bash
git push
git push -u origin mi-rama   # primera vez
```

El `-u` enlaza la rama local con la remota.

---

## Push rechazado

```console
! [rejected] master -> master (fetch first)
```

El remoto tiene commits que vos no tenés.
Solución: `git pull`, resolver, `git push`.

---

## Ciclo de trabajo

1. `git pull`
2. Trabajar y confirmar
3. `git pull`
4. `git push`

Nunca fuerces un envío para "arreglar" un rechazo.

---

## ¿Qué es revertir?

- Crea un commit **nuevo** que deshace otro.
- El historial no se reescribe: solo crece.
- Es la opción segura en un remoto compartido.

---

## Revertir y propagar

```bash
git revert --no-edit <commit>
git push
```

Las demás personas lo reciben con `git pull`.

---

## Autenticación: si falla

| Mensaje | Causa |
|---|---|
| `Authentication failed` | Token vencido o mal escrito |
| `403 ... denied` | Sin permiso de escritura |
| `Repository not found` | URL mala o repositorio privado |

---

## Actividad práctica

Taller 01: dos copias de tu `diario`.

- Clonar, enviar, descargar.
- Provocar y resolver un rechazo.
- Revertir un commit publicado.

---

## Resumen

- `clone` copia; `pull` baja; `push` sube.
- Rechazo = primero `pull`.
- `revert` corrige sin borrar historia.

---

## Evaluación

Quiz 01: 8 ítems sobre clonar, pull, push y revert.
