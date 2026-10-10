# Soluciones — Quiz 01 (material docente)

No compartir este archivo con los estudiantes.

---

**1.** Respuesta correcta: **B**.
Justificación: un clon trae los archivos, todo el historial y las ramas remotas, y deja
`origin` configurado. No es solo la última versión (A), no queda vacío (C) y no se
limita a una rama (D).

**2.** Respuesta correcta: **B**.
Justificación: `pull` va del remoto al local; `push` va del local al remoto. Ninguno de
los dos borra commits por sí mismo.

**3.** Respuesta correcta: **C**.
Justificación: `(fetch first)` indica que el remoto contiene trabajo que el local no
tiene. Un problema de permisos (A) produciría un error 403 o de autenticación, no un
rechazo de este tipo; un remoto inexistente (B) daría `Repository not found`; los
archivos sin confirmar (D) no afectan al push.

**4.** Respuesta correcta: **A**.
Justificación: `git revert` crea un commit nuevo que deshace el error y se publica con
`push`, sin modificar commits ya publicados. B reescribe historial compartido, C no
corrige el remoto y D no tiene efecto sobre el error.

**5.** Respuesta esperada: dos líneas, una `(fetch)` y otra `(push)`, con `origin` y la
URL `https://github.com/docente-git/proyecto-ejemplo.git`.
Justificación: `git clone` configura automáticamente el remoto de origen, por eso no
hizo falta `git remote add`.

**6.** Respuesta esperada: el remoto tiene commits que el repositorio local no tiene
(alguien publicó antes), por eso Git no permite el envío. Siguiente comando:
`git pull` (resolver un conflicto si aparece) y luego repetir `git push`.

**7.** Respuesta esperada: forzar el envío (`--force`) sobrescribe en el remoto los
commits de la otra persona, que se perderían. La secuencia correcta es `git pull`,
resolver el conflicto si lo hay (editar, `git add`, `git commit`) y `git push`.
Justificación: un rechazo no es un error que haya que "saltarse": avisa que falta
integrar trabajo ajeno.

**8.** Respuesta esperada (admite variantes equivalentes):

```bash
git log --oneline                  # (a) localizar el commit y copiar su identificador
git revert --no-edit <commit>      # (b) crear el commit de reversión
git push                           # (c) publicar la corrección
# (d) la otra persona, en su copia:
git pull
```

Justificación: la reversión es un commit nuevo; al publicarla, las demás copias la
reciben con un `pull` normal.
