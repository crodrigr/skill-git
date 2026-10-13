# Básico 01 — Resolver un conflicto en dos archivos

## Estado inicial

Una copia del repositorio `diario` en `master` con dos archivos rastreados: `diario.txt`
y `configuracion.txt`. Existe una rama `ajustes` que partió del mismo commit y que
modificó la línea 2 de **ambos** archivos. `master`, por su parte, también modificó esas
mismas líneas de los dos archivos, con otro contenido. Estás parado en `master`.

## Proceso esperado

1. Fusionar la rama `ajustes` en `master`.
2. Identificar, desde el estado del repositorio, qué archivos están en conflicto y cuáles
   se fusionaron solos (si hay alguno).
3. Resolver `diario.txt` **combinando** ambos lados, y `configuracion.txt` conservando
   **solo el lado entrante**.
4. Comprobar que no queda ningún marcador de conflicto en ningún archivo.
5. Marcar los archivos como resueltos y completar la fusión.
6. Mostrar el historial con el gráfico de la fusión.

## Resultado esperado

- Tu archivo `diario.txt` incluye la información de ambos lados y `configuracion.txt` tiene
  el valor de la rama `ajustes`.
- Ningún archivo contiene `<<<<<<<`, `=======` ni `>>>>>>>`.
- `git status` queda limpio y el historial muestra un commit de fusión con dos padres.
- Podés explicar qué es cada letra de `git status -s` para los archivos en conflicto.

## Restricciones

- No usar `git merge --abort` ni rehacer la fusión desde cero.
- Resolver los marcadores editando los archivos; no copiar un archivo entero de otra rama.
- El mensaje del commit de fusión debe ser descriptivo.

## Resultados de aprendizaje

- RA-1: Resolver los conflictos que surgen al trabajar con repositorios locales y remotos.
