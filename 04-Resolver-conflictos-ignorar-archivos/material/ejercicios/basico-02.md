# 🟢 Básico 02 — Crear un .gitignore

> 🟢 **Nivel:** Básico · 🎓 **Resultados de aprendizaje:** RA-2

## 🧱 Estado inicial

La carpeta `proyecto-gitignore`, con un repositorio recién iniciado y estos archivos, sin
confirmar: `README.md`, `src/programa.txt`, `build/salida.txt`, `registro.log`,
`depuracion.log`, `auditoria.log`, `dependencias/libreria.txt` y `credenciales.txt` (con
contenido ficticio, **NO REAL**).

## 🛠️ Proceso esperado

1. Revisar el estado del repositorio y anotar qué archivos aparecen como no rastreados.
2. Crear un `.gitignore` que ignore:
   - la carpeta de salida generada,
   - todos los archivos con extensión `.log`, **excepto** `auditoria.log`,
   - la carpeta de dependencias,
   - el archivo de credenciales.
3. Agregar al archivo un comentario que explique cada grupo de patrones.
4. Comprobar que el estado del repositorio solo muestra lo que debe versionarse.
5. Confirmar los archivos y listar lo que quedó rastreado.

## ✅ Resultado esperado

- ☑️ Tras crear el `.gitignore`, el estado del repositorio muestra únicamente `.gitignore`,
  `README.md`, `auditoria.log` y `src/` como no rastreados.
- ☑️ El primer commit incluye `.gitignore` y no incluye `build/`, `dependencias/`, los
  `.log` ignorados ni `credenciales.txt`.
- `auditoria.log` sí está rastreado.
- ☑️ Podés indicar para qué sirve cada tipo de patrón que usaste (nombre exacto, comodín,
  carpeta, excepción).

## 🚧 Restricciones

- 🚫 No usar `git add -f`.
- 🚫 No borrar ningún archivo del disco.
- 🚫 Cada patrón debe ir en su propia línea, con comentarios que expliquen los grupos.

## 🎓 Resultados de aprendizaje

- RA-2: Evitar que ciertos archivos sean rastreados por Git, y corregir el caso de los ya rastreados.
