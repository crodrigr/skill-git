# Taller 01 — Colaborar con dos copias de tu diario

## Objetivo

Aplicar en un proyecto real los cuatro resultados de la clase: clonar (RA-1),
descargar cambios (RA-2), enviar cambios (RA-3) y revertir un commit publicado (RA-4).

## Contexto

Seguirás trabajando sobre el repositorio `diario` que creaste en la Clase 02 y que ya
está en tu cuenta de GitHub. Para simular que dos personas colaboran, tendrás dos copias
locales del mismo repositorio en carpetas distintas (copia A y copia B).

## Pasos

1. Verificá que tu token de acceso a GitHub sigue vigente y configurá
   `pull.rebase false`.
2. Cloná el repositorio de ejemplo que te indique el docente y explorá su historial y
   sus ramas.
3. Cloná tu repositorio `diario` en dos carpetas distintas (copia A y copia B).
4. En la copia A, hacé un commit y enviálo con push.
5. En la copia B, descargá los cambios con pull y comprobá que llegó el commit.
6. Hacé un commit en cada copia **sin sincronizar entre ellas**, publicá primero desde
   una, provocá el rechazo en la otra y resolvelo con pull y un nuevo push.
7. Elegí un commit ya publicado (o creá uno con un dato incorrecto y publicalo),
   revertilo, enviá la reversión y comprobá que la otra copia la recibe con pull.

## Entregable

Tu repositorio `diario` en GitHub con un commit de reversión publicado, y dos copias
locales sincronizadas con el remoto (`git status` en ambas debe indicar que están al día
con `origin`).

## Criterios de evaluación

- Ambos repositorios se clonaron correctamente y el remoto de origen está configurado.
- El pull trajo los cambios sin pérdida de commits.
- El push rechazado se resolvió con pull y no con reescritura de historial.
- La reversión se publicó como commit nuevo y el historial previo se conserva.
