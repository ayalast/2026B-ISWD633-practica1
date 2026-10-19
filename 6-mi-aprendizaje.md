# Mi aprendizaje

Al principio no entendía bien qué pasaba con la información al borrar un contenedor. Cuando borré el de WordPress el sitio dejó de cargar, pero los archivos seguían en la carpeta `www`. Al crearlo otra vez con el mismo montaje y la misma base de datos seguían la entrada y el color de fondo. No me pidió instalar WordPress de nuevo.

Con un bind mount puedo usar una carpeta de mi PC. En un volumen nombrado Docker se encarga de guardar los archivos. En los dos casos puedo reemplazar el contenedor si conservo los datos y uso el mismo montaje. Un volumen anónimo tambien puede conservar los datos, pero necesito su identificador para volver a usarlo.

Las redes `net-wp` y `net-drupal` permiten que los servicios se encuentren por el nombre del contenedor. Por eso WordPress se conecta a `cp3` y Drupal a `server-postgres`. Para entrar desde el navegador de Windows se publica un puerto como `9500:80`. Entre los contenedores se usan los puertos internos.

En Drupal hubo que usar `/var/lib/postgresql` para PostgreSQL 18 y activar `pg_trgm`. pgAdmin quedó en el puerto 9600 porque el 9500 ya estaba ocupado por WordPress. Con `docker volume inspect` se revisó dónde se guardaban los archivos. También se vio que detener un contenedor no basta para borrar el volumen si todavía está asociado a él.
