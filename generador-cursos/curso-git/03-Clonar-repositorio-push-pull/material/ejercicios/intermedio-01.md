# Intermedio 01 — Enviar cambios con push y resolver un rechazo

## Estado inicial

Las mismas dos copias de `diario` (A tuya, B de otra persona), sincronizadas con el
remoto en `master`. `pull.rebase` es `false`.

## Proceso esperado

1. En la copia A, confirmar un cambio y enviarlo al remoto. Comprobar que el remoto lo
   refleja (por ejemplo, desde la copia B).
2. En la copia A, crear una rama nueva con un commit propio y enviarla **por primera
   vez**; verificar que queda enlazada con su rama remota.
3. Provocar un rechazo: en la copia B, confirmar y publicar un cambio en `master`;
   luego, en la copia A (sin descargarlo), confirmar un cambio en `master` y
   intentar enviarlo.
4. Anotar el mensaje de Git, resolver la situación sin reescribir historial y lograr
   que el envío se complete.
5. Mostrar el historial final.

## Resultado esperado

- El remoto contiene el commit del paso 1 y la rama nueva del paso 2, con la rama local
  enlazada a la remota.
- El envío del paso 3 es rechazado y el mensaje se interpreta correctamente: el remoto
  tiene trabajo que la copia A no tiene.
- Tras resolverlo, el remoto contiene los commits de ambas copias y ninguno se perdió.

## Restricciones

- Los dos cambios del paso 3 deben tocar **archivos distintos** (así el rechazo se
  resuelve sin conflicto).
- Está prohibido forzar el envío.

## Resultados de aprendizaje

- RA-3: Enviar cambios del repositorio local al repositorio remoto.
- RA-2: Descargar actualizaciones del repositorio remoto al repositorio local.
