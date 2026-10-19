# Mi aprendizaje

Al principio no tenía claro qué pasaba con la información al borrar un contenedor. En WordPress pude comprobarlo: eliminé el contenedor, el sitio dejó de cargar y los archivos seguían en la carpeta `www`. Al recrearlo con el mismo montaje y la misma base de datos, se conservaron la entrada y el color de fondo. No fue necesario instalar WordPress otra vez.

Un bind mount permite trabajar con una carpeta concreta de mi PC. Un volumen nombrado deja que Docker gestione el almacenamiento. En ambos casos, el contenedor se puede reemplazar si se conservan los datos y se vuelve a configurar el montaje. Un volumen anónimo también puede persistir, pero hay que conocer su identificador para reutilizarlo.

Las redes `net-wp` y `net-drupal` permiten conectar los servicios usando el nombre del contenedor. Por eso WordPress usa `cp3` como host de MySQL y Drupal usa `server-postgres`. Los puertos publicados, como `9500:80`, permiten entrar desde el navegador de Windows; la comunicación interna usa los puertos del contenedor.

En la configuración de Drupal se ajustó el montaje de PostgreSQL 18 a `/var/lib/postgresql` y se habilitó `pg_trgm`. pgAdmin quedó en el puerto 9600 porque WordPress ya usaba el 9500. Para revisar los montajes se usó `docker volume inspect`. También se comprobó que detener un contenedor no basta para borrar un volumen que todavía está asociado a él.
