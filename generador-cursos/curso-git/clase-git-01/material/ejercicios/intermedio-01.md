# Intermedio 01 — Flujo local completo

## Estado inicial

Una carpeta nueva y vacía en tu computadora, con Git ya instalado y configurado.

## Proceso esperado

1. Inicializar un repositorio en esa carpeta.
2. Crear un archivo de texto cualquiera con algún contenido.
3. Prepararlo con el área de staging y confirmarlo en un commit con un mensaje
   descriptivo.
4. Modificar ese mismo archivo (agregar o cambiar una línea).
5. Preparar y confirmar ese segundo cambio en un nuevo commit, con otro mensaje
   descriptivo distinto al primero.
6. Crear un segundo archivo y confirmarlo en un tercer commit.

## Resultado esperado

- El repositorio tiene al menos 3 commits.
- Cada commit tiene un mensaje descriptivo propio (no genérico como "cambios").
- `git status` no muestra cambios pendientes al finalizar (todo quedó confirmado).

## Restricciones

- No uses `git commit -m "cambios"`, `"update"` ni mensajes igual de genéricos.
- Cada commit debe representar un cambio real y distinto de los anteriores.

## Resultados de aprendizaje

- RA-4: Crear repositorios locales.
- RA-5: Gestionar de forma efectiva los cambios mediante el área de staging.
