# 🟡 Intermedio 02 — Dejar de rastrear un archivo y averiguar qué regla lo ignora

> 🟡 **Nivel:** Intermedio · 🎓 **Resultados de aprendizaje:** RA-2

## 🧱 Estado inicial

Un repositorio `proyecto-gitignore` en el que, por descuido, ya se confirmó todo en el
primer commit: `README.md`, `src/programa.txt`, `build/salida.txt`, `registro.log` y
`credenciales.txt` (contenido ficticio, **NO REAL**). Todavía no existe `.gitignore`. Hay
además un archivo `depuracion.log` sin confirmar.

## 🛠️ Proceso esperado

1. Crear un `.gitignore` que ignore la carpeta de salida, los archivos `.log` y el archivo
   de credenciales.
2. Modificar `credenciales.txt` y observar si Git lo sigue viendo cambiar. Anotar por qué.
3. Dejar de rastrear `credenciales.txt`, `build/salida.txt` y `registro.log` **sin
   borrarlos de tu carpeta**.
4. Confirmar el cambio junto con el `.gitignore`.
5. Averiguar, para `credenciales.txt` y `depuracion.log`, qué archivo y qué línea del
   `.gitignore` los ignora.
6. Explicar por escrito qué pasaría si `credenciales.txt` ya se hubiera publicado en el
   remoto antes de dejar de rastrearlo.

## ✅ Resultado esperado

- ☑️ Después del paso 4, `git status` queda limpio y los archivos siguen existiendo en la
  carpeta de trabajo.
- ☑️ Los archivos ignorados ya no aparecen en la lista de archivos rastreados.
- ☑️ La respuesta del paso 5 identifica el archivo, el número de línea y el patrón.
- ☑️ La explicación del paso 6 menciona que el historial conserva las versiones anteriores y
  que la credencial debe reemplazarse.

## 🚧 Restricciones

- 🚫 No borrar archivos del disco ni usar `git rm` sin la opción que conserva el archivo.
- 🚫 No usar credenciales reales.
- 🚫 No intentar reescribir el historial.

## 🎓 Resultados de aprendizaje

- RA-2: Evitar que ciertos archivos sean rastreados por Git, y corregir el caso de los ya rastreados.
