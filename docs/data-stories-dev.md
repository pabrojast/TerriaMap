# Data Stories en dev

El cliente usa Nominatim a través de `/data-stories/api/location-search` (CKAN): búsquedas explícitas, caché y límite compartido. Mantener `wwwroot/config.json` y los valores Helm sincronizados. El ConfigMap activo también debe contener este proveedor.

La entrega de 2026-09-11 se compiló con modificaciones locales de `terriajs-1` (cola de escenas y búsqueda explícita). Desde 2026-09-18 esos cambios están publicados en el fork (`c2d5a0af3`) y la revisión fijada en `package.json` los incluye, así que un checkout limpio reproduce la imagen con `yarn gulp release --baseHref=/terria/`.

Actualizar CKAN antes de activar el proveedor; después cambiar la imagen de Terria. Fijar las imágenes por digest y guardar la configuración anterior. Los shares conservan sus instancias originales, pero `ckanext.data_stories.terria_runtime_url` permite probarlos con el visor de dev. Producción no forma parte de este despliegue.

## Narrative images and visual templates (2026-09-24)

The pinned TerriaJS revision enables authenticated image upload/paste/drop through the same-origin `storyImageUploadUrl: /story-images/upload`. CKAN Pages supplies session/CSRF/size limits and persistent storage; failed uploads retain the editor draft. TinyMCE image dialogs appear above StoryBuilder, with proportional images and a scrollable editor on small screens.

Feature Info grows with late COG/table/media content until the viewport limit, preserving manual resizing and a Reset automatic size button. Shared state records `sizeMode` and keeps legacy dimensions compatible.

CKAN Pages owns combined text/map/dashboard templates and scroll/manual-slide navigation. Dashboard Builder accepts versioned same-origin narrative filter/highlight commands; map references reuse Terria's applyScene receipt/completion contract. Deploy this app with the dev workflow in `ckan-unesco-docker`, passing the immutable TerriaMap commit. Check the CKAN Stories release alongside it; this config requires the new Pages upload GET endpoint.

## Personal image library

Set `storyImageLibraryUrl: /story-images/library` on the same CKAN origin. The new Pages backend and its migration must be available before deploying this configuration. CKAN owns the reusable library, validation and optimization; Terria stores permanent portal URLs. Existing shares remain readable and inline images are converted when a story step is edited and saved.
