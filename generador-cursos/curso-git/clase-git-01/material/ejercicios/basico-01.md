# Básico 01 — Instalar y configurar Git

## Estado inicial

Tu propia computadora, con o sin Git instalado previamente.

## Proceso esperado

1. Verificar si Git ya está instalado con `git --version`. Si no lo está, instalarlo
   siguiendo las instrucciones de tu sistema operativo.
2. Configurar tu identidad con `git config --global user.name` y
   `git config --global user.email`, usando tu propio nombre y correo.
3. Verificar que la configuración quedó guardada.

## Resultado esperado

- `git --version` muestra una versión de Git instalada (2.x o superior).
- `git config --list` muestra tu `user.name` y `user.email` correctos.

## Restricciones

- No uses datos de otra persona como identidad: usa tu propio nombre y correo.
- Si ya tenías Git instalado con una versión antigua, no hace falta reinstalarlo:
  alcanza con confirmar que `git --version` responde.

## Resultados de aprendizaje

- RA-2: Instalar Git en su entorno de desarrollo.
- RA-3: Configurar su identidad en Git, incluyendo el nombre y el correo electrónico.
