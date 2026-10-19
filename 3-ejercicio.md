## Esquema para el ejercicio
![Imagen](esquema-ejercicio3.PNG)

### Crear red net-wp
docker network create net-wp

### Para que persista la información es necesario conocer en dónde mysql almacena la información.
# COMPLETAR LA SIGUIENTE ORACIÓN. REVISAR LA DOCUMENTACIÓN DE LA IMAGEN EN https://hub.docker.com/
En el esquema del ejercicio carpeta del contenedor (a) es /var/lib/mysql

Ruta carpeta host: C:\Users\USER\Desktop\Practicas-Docker\practica1\ejercicio3\db

### ¿Qué contiene la carpeta db del host?
En este momento se encuentra vacía ya que la acabo de crear para usarla junto al contenedor mysql.

### Crear un contenedor con la imagen mysql:8  en la red net-wp, configurar las variables de entorno: MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER y MYSQL_PASSWORD
docker run -d --name cp3 -v C:\Users\USER\Desktop\Practicas-Docker\practica1\ejercicio3\db:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123 -e MYSQL_DATABASE=db1 -e MYSQL_USER=bryan -e MYSQL_PASSWORD=123 --network net-wp mysql:8

### ¿Qué observa en la carpeta db que se encontraba inicialmente vacía?
Se acaba de llenar con archivos propios de la aplicacion de mysql, donde se guardarán los datos en un futuro.

### Para que persista la información es necesario conocer en dónde wordpress almacena la información.
# COMPLETAR LA SIGUIENTE ORACIÓN. REVISAR LA DOCUMENTACIÓN DE LA IMAGEN EN https://hub.docker.com/
En el esquema del ejercicio la carpeta del contenedor (b) es /var/www/html

Ruta carpeta host: C:\Users\USER\Desktop\Practicas-Docker\practica1\ejercicio3\www

### Crear un contenedor con la imagen wordpress en la red net-wp, configurar las variables de entorno WORDPRESS_DB_HOST, WORDPRESS_DB_USER, WORDPRESS_DB_PASSWORD y WORDPRESS_DB_NAME (los valores de estas variables corresponden a los del contenedor creado previamente)

docker run -d --name wordpress -v "C:\Users\USER\Desktop\Practicas-Docker\practica1\ejercicio3\www:/var/www/html" -e WORDPRESS_DB_HOST=cp3 -e WORDPRESS_DB_USER=bryan -e WORDPRESS_DB_PASSWORD=123 -e WORDPRESS_DB_NAME=db1 --network net-wp -p 9500:80 wordpress

### Personalizar la apariencia de wordpress y agregar una entrada
Cambié el color de fondo del sitio y publiqué una entrada. Comprobé que el cambio de color se veía al recargar la página.

### Eliminar el contenedor y crearlo nuevamente, ¿qué ha sucedido?
Eliminé y volví a crear el contenedor de WordPress y todo se mantuvo igual: la entrada y el color de fondo seguían ahí y no me pidió instalarlo de nuevo. Esto sucede porque los archivos están en mi carpeta www mediante el bind mount y los datos del sitio permanecen en MySQL. Por eso puedo reemplazar el contenedor si uso los mismos montajes y la misma base de datos.
