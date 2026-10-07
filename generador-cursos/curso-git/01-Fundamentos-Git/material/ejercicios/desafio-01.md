# Desafío 01 — Un repositorio de principio a fin

## Estado inicial

Una carpeta nueva y vacía en tu computadora, con Git instalado y configurado.

## Proceso esperado

1. Elegir un proyecto propio simple (por ejemplo, una lista de tareas en un archivo de
   texto, o cualquier otra idea corta).
2. Inicializar un repositorio en una carpeta nueva para ese proyecto.
3. Hacer al menos **cuatro** commits significativos a medida que el proyecto avanza
   (no todos al final): cada uno debe representar un cambio real, con un mensaje
   descriptivo propio.
4. Crear un `README.md` que describa el proyecto (qué es, cómo se usa) y confirmarlo
   en uno de esos commits.
5. Usar `git log` para listar el historial completo y `git show` (o `HEAD~N`) para
   identificar y describir, por escrito, el contenido de un commit intermedio
   específico (ni el primero ni el último).

## Resultado esperado

- Un repositorio local con al menos 4 commits, cada uno con un mensaje descriptivo
  distinto.
- Un `README.md` presente y confirmado en el historial.
- Una descripción escrita, correcta, de qué cambió en un commit intermedio elegido.

## Restricciones

- No se permite un único commit gigante al final: los commits deben reflejar el avance
  real del trabajo.
- Ningún mensaje de commit puede ser genérico ("cambios", "update", "final").

## Resultados de aprendizaje

- RA-1: Comprender qué es el control de versiones y por qué es importante.
- RA-4: Crear repositorios locales.
- RA-5: Gestionar de forma efectiva los cambios mediante el área de staging.
- RA-6: Crear y manejar commits, y navegar entre ellos.
- RA-7: Crear y redactar un archivo README.md.

## Casos de prueba

| Acción | Resultado esperado |
|---|---|
| `git log --oneline` al finalizar | Al menos 4 líneas (4 commits), cada una con un mensaje distinto y descriptivo |
| `git show` sobre el commit que incluye el README | Muestra `README.md` como archivo agregado, con su contenido |
| `git show`/`HEAD~N` sobre un commit intermedio elegido | El contenido mostrado coincide exactamente con la descripción escrita por el estudiante |
