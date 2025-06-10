# curso-de-docker-fundamentos


claseDocker/
├── Dockerfile
├── sitio/
│   ├── index.html
│   └── style.css



## Construye una imagen 

-t viene de "tag".
Le da un nombre personalizado a la imagen que estás construyendo. En este caso: sitio.

El punto (.) representa el directorio de contexto de construcción. Es decir, le dice a Docker:
    "Construye esta imagen usando el Dockerfile y los archivos que están en este directorio actual."
    
Puedes usar fácilmente esa imagen después con:
$ docker build -t mi-sitio .

usar etiquetas versionadas:
$ docker build -t mi-sitio:1.0.0 .

| Parte               | Significado                                                                                                                                                  |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `docker build`      | Comando para construir una **imagen Docker** a partir de un `Dockerfile`.                                                                                    |
| `-t mi-sitio:1.0.0` | Etiqueta (`tag`) para darle nombre y versión a la imagen que estás construyendo. En este caso: `mi-sitio` es el nombre y `1.0.0` es la versión.              |
| `.`                 | Indica el **directorio actual** como el *contexto de construcción*, es decir, donde está el `Dockerfile` y los archivos necesarios para construir la imagen. |



# Ejecutar el contenedor

$ docker run -it --rm -d -p 8080:80 --name sito-web mi-sitio:1.0.0


#Luego abre tu navegador y entra a:

http://localhost:5000



# Muestra una lista de todas las imágenes que tienes almacenadas localmente en tu máquina.

$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    4bb46517cac3   2 weeks ago    142MB
mi-sitio     latest    a1b2c3d4e5f6   3 minutes ago  25MB


#" Muestra una lista de los contenedores que están corriendo actualmente (activos).
$ docker ps
CONTAINER ID   IMAGE       COMMAND                  STATUS          PORTS                  NAMES
123abc456def   mi-sitio    "/docker-entrypoint.…"   Up 5 minutes    0.0.0.0:8080->80/tcp   pedantic_goldberg


## Borrar todas las imágenes sin contenedores:
$ docker image prune -a
$ docker rmi -f mi_app:latest
$ docker rmi mi_app:latest


## Borrar todas las imágenes sin contenedores:
$ docker image prune -a

# buscar imagenes
$ docker images mi-sitio2
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
mi-sitio2    1.0.0     520df20299eb   14 hours ago   279MB

# buscar imagenes por filtro de tag
$ docker images --filter=reference='*:1.0.0'
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
mi-sitio2    1.0.0     520df20299eb   14 hours ago   279MB
mi-sitio     1.0.0     c6f1e28616a8   14 hours ago   279MB




## Si la imagen está siendo usada por algún contenedor:

# 1 Lista contenedores con:
docker ps -a

# 2 Detén y borra los contenedores relacionados:
docker stop <container_id>
docker rm <container_id>

# 3 Luego borra la imagen.



docker run -it --rm -d -p 8080:80 --name sito-web mi-sitio:1.0.0


| Opción            | Significado                                                                |
| ----------------- | -------------------------------------------------------------------------- |
| `docker run`      | Ejecuta un nuevo contenedor.                                               |
| `-it`             | Modo interactivo + terminal (útil para algunos shells o debug).            |
| `--rm`            | Elimina automáticamente el contenedor al detenerse.                        |
| `-d`              | Ejecuta el contenedor en segundo plano (modo *detached*).                  |
| `-p 8080:80`      | Mapea el puerto 80 del contenedor al 8080 de tu máquina.                   |
| `--name sito-web` | Le da al contenedor el nombre `sito-web`.                                  |
| `mi-sitio:1.0.0`  | Es la imagen que estás usando para crear el contenedor (nombre + versión). |


#El comando docker tag no duplica la imagen, solo le da otro nombre y/o etiqueta (tag) a la misma imagen ya existente.

docker tag <imagen-origen> <nuevo-nombre>:<nuevo-tag>
$ docker tag mi-sitio:1.0.0 sitio-web:v1

#  El IMAGE ID se muestra completo como un sha256 largo, lo cual es útil si necesitas usar el hash exacto (por ejemplo para hacer inspecciones o debugging).

docker images --no-trunc

REPOSITORY   TAG     IMAGE ID                                                              CREATED        SIZE
mi-sitio     latest  sha256:a1b2c3d4e5f6a7b8c9d0e1f2g3h4i5j6k7l8m9n0o1p2q3r4s5t6u7v8w9x0y1z2  5 minutes ago  25MB

#renombra el nombre la imagen
$ docker image tag sitio-web:v1 amain/sitio-web:v1

#borrar una imagen
$ docker rmi amain/sitio-web:v1
Untagged: amain/sitio-web:v1


Lista TODOS los contenedores, incluyendo los que están detenidos.
$ docker ps -a

Igual que docker ps (solo contenedores activos) pero además muestra el tamaño del contenedor:
$ docker ps --size

Lista los contenedores EN EJECUCIÓN.
$ docker ps


#detener
$ docker stop fa8f6095aa0f
fa8f6095aa0f

👉 Muestra en tiempo real el uso de recursos (CPU, memoria, red, disco) de tus contenedores activos.
$ docker stats

