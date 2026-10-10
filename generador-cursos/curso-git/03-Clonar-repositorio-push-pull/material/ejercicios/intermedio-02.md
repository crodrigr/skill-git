# Intermedio 02 — Revertir un commit y propagarlo

## Estado inicial

Las dos copias de `diario` (A tuya, B de otra persona), sincronizadas con el remoto en
`master`. Tu historial es corto y limpio.

## Proceso esperado

1. En la copia A, agregar al archivo una línea con un dato **incorrecto**, confirmarla
   y enviarla al remoto.
2. Darte cuenta del error: localizar ese commit en el historial.
3. Revertirlo de modo que el archivo vuelva a su contenido anterior y el historial
   conserve el commit original.
4. Enviar la corrección al remoto.
5. En la copia B, descargar los cambios y comprobar que recibió la corrección.

## Resultado esperado

- El historial de la copia A tiene dos commits consecutivos: el erróneo y su
  reversión; el archivo ya no contiene la línea incorrecta.
- El remoto contiene ambos commits.
- La copia B tiene el archivo corregido tras descargar y puede explicar por qué recibió
  la corrección sin conflictos.

## Restricciones

- Se debe usar la herramienta que **crea un commit nuevo** para deshacer el error.
- Está prohibido borrar commits del historial o forzar el envío.

## Resultados de aprendizaje

- RA-4: Revertir cambios en el repositorio local y en el remoto.
