# BIND MOUNT
En un bind mount mapeamos (montar) un directorio o archivo específico del sistema de archivos del host con una parte del sistema de ficheros del contenedor.

```
docker run -d --name <nombre contenedor> -v <ruta carpeta host>:<ruta carpeta contenedor> <imagen> 
```
ó
```
docker run -d --name <nombre contenedor> --mount type=bind,source=<ruta carpeta host>,target=<ruta carpeta contenedor> <imagen>
```
- destination, dst, target: La ruta donde se monta el archivo o directorio en el contenedor.
- source, src: El origen del montaje.
  
### En tu computador crear una carpeta llamada nginx y dentro de esta carpeta crea otra llamada html. Como se aprecia en la figura.
![Volúmenes](directorio.PNG)

### Crear un contenedor con la imagen nginx:alpine, mapear todos por puertos, para la ruta carpeta host colocar el directorio en donde se encuentra la carpeta html en tu computador y para la ruta carpeta contenedor: /usr/share/nginx/html (esta ruta se obtiene al revisar la documentación de la imagen)
![Volúmenes](volumen-host.PNG)
docker run -d --name cp1 -v "C:\Users\USER\Desktop\Practicas-Docker\practica1\nginx\html:/usr/share/nginx/html" -p 8080:80 nginx:alpine

### ¿Qué sucede al ingresar al servidor de nginx?
Nos aparece una pantalla con el error 403, junto con la versión de la aplicación nginx/1.31.7.

### ¿Qué pasa con el archivo index.html del contenedor?
El index.html original de la imagen quedó oculto por el bind mount. Al montar nuestra carpeta html sobre /usr/share/nginx/html, Nginx muestra los archivos de esa carpeta del host. El archivo original no se borra de la imagen.

### Ir a https://html5up.net/ y descargar un template gratuito, descomprirlo dentro de tu computador en la carpeta html
### ¿Qué sucede al ingresar al servidor de nginx?
Ahora carga el index.html que incluía el paquete descargado.

### Eliminar el contenedor
docker stop cp1
docker rm cp1

### ¿Qué sucede al crear nuevamente un contenedor montado al directorio definidos anteriormente?
Ahora directamente se crea y cargan los contenidos del paquete descargado ya que se encuentran listos en la carpeta que nginx como servicio buscará para cargar.


