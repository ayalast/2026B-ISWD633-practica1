## VOLUMEN ANÓNIMO
Un volumen anónimo se crea sin que le demos un nombre y Docker le pone un identificador largo. Sus datos pueden seguir ahí después de borrar el contenedor. No se borra solo por ser anónimo.
**Considerar**
Conviene guardar su identificador si se quiere volver a usar. El identificador se consulta con `docker inspect`.

```
docker run -d --name <nombre contenedor> -v <ruta carpeta contenedor> <nombre imagen>
```
ó
```
docker run -d --name <nombre contenedor> --mount type=volume,target=<ruta carpeta contenedor> <nombre_imagen>
```
destination, dst, target: La ruta donde se monta el archivo o directorio en el contenedor.

### Prueba realizada

```powershell
docker run -d --name anon-nginx -v /usr/share/nginx/html nginx:alpine
docker exec anon-nginx sh -c 'echo dato-del-volumen-anonimo > /usr/share/nginx/html/prueba.txt'
docker inspect anon-nginx --format '{{range .Mounts}}{{.Name}}{{end}}'
docker stop anon-nginx
docker rm anon-nginx
```

El volumen que se creó fue `fcb7a21b4ea264d1d80c030a034069742015c019eafca46e2d74f02922ad0386`. Después de borrar el contenedor seguía apareciendo en `docker volume ls`. Se usó en otro contenedor para revisar si el archivo seguía ahí

```powershell
docker run --rm -v fcb7a21b4ea264d1d80c030a034069742015c019eafca46e2d74f02922ad0386:/datos:ro nginx:alpine cat /datos/prueba.txt
```

La salida fue `dato-del-volumen-anonimo`.


### Para eliminar el contenedor y el volumen
```
docker run -d --name anon-nginx-rmv -v /usr/share/nginx/html nginx:alpine
docker rm -fv anon-nginx-rmv
```
_esta instrucción elimina el volumen si éste es de tipo anónimo_

En esta segunda prueba sí se borró el volumen junto con el contenedor. La opción `-v` de `docker rm` no borra los volúmenes nombrados.

# Eliminar todos los volúmenes anónimos (dangling volumes) no utilizados
Un volumen sin usar no está referenciado por ningún contenedor, ni siquiera uno detenido. Puede quedar así al borrar un contenedor sin borrar su volumen.

Antes de limpiar se revisó qué volúmenes estaban sin usar

```powershell
docker volume ls --filter dangling=true
```

Solo aparecía el volumen anónimo de la primera prueba. En esta versión de Docker, `docker volume prune` elimina por defecto los volúmenes anónimos sin usar. Con `--all` también incluiría los nombrados sin usar.
```
docker volume prune
```

La limpieza borró el volumen de la primera prueba y liberó 1.418 kB. Los seis volúmenes nombrados de PostgreSQL, pgAdmin y Drupal siguieron ahí.
