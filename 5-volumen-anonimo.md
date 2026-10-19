## VOLUMEN ANÓNIMO
Un volumen anónimo es un volumen gestionado por Docker que se crea sin que le demos un nombre. Docker le asigna un identificador largo. Puede conservar datos después de eliminar el contenedor; no se borra automáticamente por ser anónimo.
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

El volumen generado fue `fcb7a21b4ea264d1d80c030a034069742015c019eafca46e2d74f02922ad0386`. Después de borrar el contenedor, seguía en `docker volume ls`. Se montó en otro contenedor para comprobar el archivo:

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

En esta segunda prueba, el volumen anónimo desapareció junto con el contenedor. La opción `-v` de `docker rm` no elimina volúmenes nombrados.

# Eliminar todos los volúmenes anónimos (dangling volumes) no utilizados
Un volumen sin usar no está referenciado por ningún contenedor, ni siquiera uno detenido. Puede quedar así al borrar un contenedor sin borrar su volumen.

Antes de limpiar se revisó la lista:

```powershell
docker volume ls --filter dangling=true
```

Solo aparecía el volumen anónimo de la primera prueba. En esta versión de Docker, `docker volume prune` elimina por defecto los volúmenes anónimos sin usar. Con `--all` también incluiría los nombrados sin usar.
```
docker volume prune
```

La limpieza eliminó el volumen de la primera prueba y liberó 1.418 kB. Los seis volúmenes nombrados de PostgreSQL, pgAdmin y Drupal permanecieron.
