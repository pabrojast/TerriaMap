# Data Stories en dev

El cliente usa Nominatim a través de `/data-stories/api/location-search` (CKAN): búsquedas explícitas, caché y límite compartido. Mantener `wwwroot/config.json` y los valores Helm sincronizados. El ConfigMap activo también debe contener este proveedor.

La entrega de 2026-09-11 se compiló con modificaciones locales de `terriajs-1` (cola de escenas y búsqueda explícita). Desde 2026-09-18 esos cambios están publicados en el fork (`c2d5a0af3`) y la revisión fijada en `package.json` los incluye, así que un checkout limpio reproduce la imagen con `yarn gulp release --baseHref=/terria/`.

Actualizar CKAN antes de activar el proveedor; después cambiar la imagen de Terria. Fijar las imágenes por digest y guardar la configuración anterior. Los shares conservan sus instancias originales, pero `ckanext.data_stories.terria_runtime_url` permite probarlos con el visor de dev. Producción no forma parte de este despliegue.
