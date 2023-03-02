---
title: Dockerizando una aplicación
has_children: false
parent: Introducción a Docker
nav_order: 2
has_toc: true
---
# Dockerizando una aplicación
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>


## Dockerfile

Para dockerizar una aplicación, necesitamos crear un archivo `Dockerfile`. Este archivo contiene instrucciones para construir una imagen de Docker para nuestra aplicación.

{: .important}
Es obligatorio que es archivo se llame `Dockerfile`.

Cuando hayamos creado el archivo, necesitamos especificar la imagen base que usaremos.

Recordemos la arquitectura por capas, y que hablamos de que un contenedor esta compuesto por varias capas de imágenes. En este caso, nuestra aplicación está escrita en Node.js, asi que vamos a usar la imagen oficial de Node.js como base:

```yaml
FROM node:18
```

Esto le indica a docker que cuando cree el contenedor, debe descargar la imagen de node.

Lo siguiente, es crear una carpeta donde estará el código fuente de nuestra aplicación, esto lo hacemos con:

```yaml
RUN mkdir -p /home/app
```

Pensemos en el contenedor como si fuera una maquina virtual. En este caso, el contenedor corre sobre una distribución de Linux, y el comando RUN en el Dockerfile ejecuta un comando en esa VM (estamos simplificando la explicación). El comando `mkdir` se ejecutara en el contenedor y crea una carpeta `app` en `home` 

Luego, debemos copiar los archivos de nuestra aplicación al contenedor:

```yaml
COPY . /home/app
```

{: .concept}
El comando `COPY`, a diferencia de `RUN`, nos permite acceder a los archivos del sistema operativo anfitrión

Lo que queda es exponer un puerto dentro del contenedor para que la app pueda interactuar hacia afuera:

```yaml
EXPOSE 3000
```

Finalmente, nos queda incluir la instrucción para levantar o correr nuestra aplicación. Recordemos que en nuestro computador usábamos la instrucción `node index.js`.

En un Dockerfile, `RUN` se utiliza para ejecutar comandos durante la construcción de la imagen. Por ejemplo, para instalar dependencias. `CMD` se utiliza para especificar el comando que se ejecutará cuando se inicie un contenedor a partir de la imagen. Por ejemplo, para ejecutar la aplicación o el servidor web que se está construyendo en la imagen.

Incluyamos esto en el Dockerfile:

```yaml
CMD ["node", "/home/app/index.js"]
```

Con esto tenemos el Dockerfile listo para crear el contenedor, pero todavía nos faltan algunos pasos.

## Redes internas

Docker tiene una función llamada "redes", que permite a los contenedores comunicarse entre sí de forma aislada del resto de la red. Esto significa que los contenedores pueden interactuar entre sí sin necesidad de exponer puertos al mundo exterior, lo que mejora la seguridad.

{: .concept}
Anteriormente tuvimos que mapear el puerto del contenedor de mongo a un puerto externo, para que el contenedor se pueda comunicar con una app que estaba en nuestra máquina.
En este caso, si tenemos dos contenedores podemos hacer que se comuniquen entre si sin necesidad de exponer los puertos. Para esto usamos las redes internas.


Por defecto, Docker crea una red interna para cada proyecto, y los contenedores que se crean dentro del proyecto se conectan automáticamente a la red interna. Los contenedores pueden comunicarse entre sí utilizando los nombres de host de los demás contenedores en la red, que son resueltos automáticamente por Docker.

Es posible crear una red personalizada para un proyecto, en lugar de utilizar la red predeterminada. Esto se puede hacer utilizando el comando `docker network create`. Una vez creada la red personalizada, los contenedores se pueden conectar a ella utilizando el comando `docker run` y especificando el nombre de la red personalizada con la opción `--network`.

En resumen, las redes internas de Docker permiten a los contenedores comunicarse entre sí de forma segura y aislada del resto de la red, lo que mejora la seguridad y la escalabilidad de las aplicaciones.

Primero usemos el comando `docker network ls` para ver todas las redes que existen:

```powershell
PS C:\Users\fersz> docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
a8c6d19a29f0   bridge    bridge    local
2224f64a2947   host      host      local
c3bd846dcd0f   none      null      local
```

Ahora, podemos usar el comando `docker network create` seguido del nombre de la red para crear una nueva:

```powershell
PS C:\Users\fersz> docker network create red_ppy
4de1ae4d4089c4c90ab740926e09ef40ea685df93168de7bfaf7b57bf463134f
PS C:\Users\fersz> docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
a8c6d19a29f0   bridge    bridge    local
2224f64a2947   host      host      local
c3bd846dcd0f   none      null      local
4de1ae4d4089   red_ppy   bridge    local
```

Con esto, ya tenemos una red lista para empezar a usar.

Finalmente, necesitamos volver al código fuente de nuestra aplicación y volver a cambiar la linea `11`.

```jsx
mongoose.connect('mongodb://ppy:password@localhost:27017/miapp?authSource=admin')
```

Asi como la dejamos, vemos que apunta a `localhost:2717`, esto significa que se va a comunicar al puerto 27017 del computador anfitrión. Esto estaba bien antes, pero ahora queremos usar la red interna.

Para arreglar esto podemos cambiar `[localhost](http://localhost)` por el nombre del contenedor de la base de datos: `mongo_ppy`

```jsx
mongoose.connect('mongodb://ppy:password@mongo_ppy:27017/miapp?authSource=admin')
```

Ahora estamos listos para crear el contenedor.

## Docker build.

Este nuevo comando nos permite crear una imagen a partir de un archivo docker file.

Tenemos que ir a la terminal y ubicarnos en la carpeta del proyecto en node, una vez ahi ejecutar la siguiente instrucción:

```powershell
docker build -t app_ppy:1 .
```

Este comando toma dos argumentos:

- `-t` es para asignarle un nombre, en este caso `app_ppy`, seguido de una etiqueta, en este caso 1, que puede ser el numero de version.
- El segundo argumento especifica la ruta donde se encuentra el dockerfile. En este caso es un `.`, porque el archivo esta en la raíz.

Una vez terminado, deberíamos ver algo asi:

```powershell
C:\Users\fersz\OneDrive\Escritorio\mongoapp-curso-docker-main>docker build -t app_ppy:1 .
[+] Building 60.2s (8/8) FINISHED
 => [internal] load build definition from Dockerfile                                          0.1s
 => => transferring dockerfile: 148B                                                          0.0s 
 => [internal] load .dockerignore                                                             0.0s 
 => => transferring context: 2B                                                               0.0s 
 => [internal] load metadata for docker.io/library/node:18                                    5.7s
 => [1/3] FROM docker.io/library/node:18@sha256:586cdef48f920dea2f47a954b8717601933aa1daa0a  52.9s
 => => resolve docker.io/library/node:18@sha256:586cdef48f920dea2f47a954b8717601933aa1daa0a0  0.0s 
 => => sha256:586cdef48f920dea2f47a954b8717601933aa1daa0a08264abf9144789abf8 1.21kB / 1.21kB  0.0s 
 => => sha256:b7483c70b94e9fbb68e91d64456ee147d120488f876d69efeae815ba164e8b 2.21kB / 2.21kB  0.0s 
 => => sha256:c8907212ebaaf68e513f6575c88613b3b679594c8345b838051c56b0097cbd 7.52kB / 7.52kB  0.0s
 => => sha256:a7655dafb1b0d7bc589fec6296a2560b3ad70e065f82d40843fdedb48b4 45.58MB / 45.58MB  50.3s 
 => => sha256:d008a3b73975d5fe29003af48a67acbe60071b84228685e627e9a56ed0b07 2.28MB / 2.28MB  18.9s 
 => => sha256:553d33218c87b9a56df091f8d2eebe8d0ba8c200784c3edc04d760f9eb26d1f6 452B / 452B    1.7s
 => => extracting sha256:a7655dafb1b0d7bc589fec6296a2560b3ad70e065f82d40843fdedb48b42b723     1.7s
 => => extracting sha256:d008a3b73975d5fe29003af48a67acbe60071b84228685e627e9a56ed0b07882     0.1s 
 => => extracting sha256:553d33218c87b9a56df091f8d2eebe8d0ba8c200784c3edc04d760f9eb26d1f6     0.0s
 => [internal] load build context                                                             0.9s
 => => transferring context: 15.31MB                                                          0.8s
 => [2/3] RUN mkdir -p /home/app                                                              0.8s
 => [3/3] COPY . /home/app                                                                    0.3s
 => exporting to image                                                                        0.3s 
 => => exporting layers                                                                       0.2s 
 => => writing image sha256:d4c7cccd880c49688e27a7089faa93f15e5ce0eb6b0312e70018271d1cdaa46b  0.0s 
 => => naming to docker.io/library/app_ppy:1                                                  0.0s 

Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them
```

Ahora podemos ver si la imagen esta creada:

```powershell
C:\Users\fersz\OneDrive\Escritorio\mongoapp-curso-docker-main>docker images  
REPOSITORY   TAG       IMAGE ID       CREATED         SIZE
app_ppy      1         d4c7cccd880c   3 minutes ago   1.01GB
node         latest    2201f1d2b46f   4 days ago      999MB
mongo        latest    a440572ac3c1   3 weeks ago     639MB
```

## Creando los contenedores dentro de la red

El contenedor anterior que creamos de mongo no estaba en la red interna, asi que tendremos que eliminarlo y crear uno nuevo. Hacemos:

```powershell
PS C:\Users\fersz> docker stop mongo_ppy
mongo_ppy
PS C:\Users\fersz> docker rm mongo_ppy
mongo_ppy
```

{: .note}
Siempre hay que detener el contenedor antes de eliminarlo


Ahora podemos crear el contenedor nuevamente, esta vez dentro de la red:

```powershell
docker create -p27017:27017 --name mongo_ppy --network red_ppy -e MONGO_INITDB_ROOT_USERNAME=ppy -e MONGO_INITDB_ROOT_PASSWORD=password mongo
```

{: .concept}
Especificamos el nombre de la red con el argumento `--network`.

Ahora podemos crear el contenedor de la aplicación:

```powershell
docker create -p3000:3000 --name app_ppy --network red_ppy app_ppy:1
```

y verificamos que estén ambos contenedores creados:

```powershell
PS C:\Users\fersz> docker ps -a
CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS    PORTS     NAMES
57bfabf51d62   app_ppy:1   "docker-entrypoint.s…"   10 seconds ago   Created             app_ppy
df8a35f840f8   mongo       "docker-entrypoint.s…"   2 minutes ago    Created             mongo_ppy
```

Lo que nos queda es arrancar los contenedores:

```powershell
PS C:\Users\fersz> docker start mongo_ppy
mongo_ppy
PS C:\Users\fersz> docker start app_ppy
app_ppy
```

{: .important}
Es importante que iniciemos primero el contenedor de mongo, porque la app va a tratar de conectarse a la base de datos.

Si todo salio bien, podemos probar la app en el navegador nuevamente:

![Untitled](images\empty.png)

![Untitled](images\ok.png)

![Untitled](images\chanchito.png)

En resumen, estos son los pasos que se siguieron para empaquetar la aplicación:

1. Creamos el Dockerfile conde incluimos la imagen de NodeJs, creamos la carpeta y copiamos los archivos. Finalmente exponemos en el puerto 3000
2. Creamos una imagen de la app con `docker build`.
3. Se creó una red personalizada para el proyecto usando el comando `docker network create`.
4. Se cambió la línea en el código fuente de la aplicación para que la aplicación pudiera conectarse a la base de datos mongo dentro de la red y no a traves del local host.
5. Se crearon los contenedores para la aplicación y la base de datos usando el comando `docker create`.
    1. Asignamos puertos
    2. Les dimos un nombre
    3. Seteamos las variables de entorno
    4. Especificamos la red
    5. Indicamos la imagen con su etiqueta
6. Se iniciaron los contenedores usando el comando `docker start`.
7. Con esto queda todo desplegado, y se probó la aplicación en el navegador.

# Docker Compose

Todos los pasos listados anteriormente hay que hacerlos por cada contenedor que usemos, y cada vez que queramos desplegarlos.

Para simplificar esto podemos usar una herramienta llamada Docker Compose, que nos permite automatizar todo esto, y ya viene incluida con docker Desktop.

Para empezar a utilizar esta herramienta debemos volver a la carpeta de nuestro proyecto, y crear un archivo llamado `docker-compose.yml`. De nuevo, el nombre del archivo debe ser exactamente igual.

Lo primero que debemos hacer es indicar la version de docker compose que vamos a utilizar. Agregamos esto al archivo:

```powershell
version: "3.9"
```

Seguidamente, vamos a agregar los servicios, o contenedores que vamos a usar:

```yaml
services:
	app_ppy:
		build: .
		ports:
      - "3000:3000"
    links:
      - mongo_ppy
```

{: .concept}
Este es un archivo YAML, y como tal las sangrias son muy importantes. Si no indentamos correctamente no va a funcionar.

Lo que hacemos en este paso es configurar el contenedor `app_py`, la palabra clave `build` le indica donde están los archivos necesarios para hacer la imagen, y luego configuramos los puertos y le indicamos con que otros contenedores se va a comunicar. 

Luego agregamos las configuraciones para el contenedor de Mongo, y el YAML tiene que quedar asi:

```yaml
version: "3.9"
services:
	app_ppy:
		build: .
		ports:
      - "3000:3000"
    links:
      - mongo_ppy
	mongo_ppy:
    image: mongo
    ports:
      - "27017:27017"
    environment:
      - MONGO_INITDB_ROOT_USERNAME=ppy
      - MONGO_INITDB_ROOT_PASSWORD=password
```

Con esto, estamos listos para continuar.

{: .important}
Es posible que si copiaste y pegaste este código, Docker Compose de un error porque no entiende algún carácter. Se resuelve borrando todos los espacios e insertando tabulaciones con tu editor de texto.

Ahora podemos ir de vuelta a la terminal, siempre ubicados en la carpeta del proyecto, y ejecutar el comando `docker compose up`. Este comando va a utilizar el archivo `docker-compose.yml` para descargar las imágenes que necesite, correr el archivo Dockerfile, que recordemos instala todas las dependencias, y crear los contenedores necesarios, ya con las redes configuradas.

Deberíamos ver algo asi en la terminal:

```powershell
C:\Users\...\mongoapp-curso-docker-main>docker compose up

[+] Running 10/10
 - mongo_ppy Pulled                                                      39.5s 
   - 10ac4908093d Pull complete                                          11.2s
   - 685504455d09 Pull complete                                          11.2s 
   - ebd36404f329 Pull complete                                          11.4s 
   - 3abd9b25affb Pull complete                                          11.4s 
   - 2d7fde532eae Pull complete                                          11.5s 
   - 24fc70e4c7d7 Pull complete                                          11.5s 
   - ffc2353072f7 Pull complete                                          11.5s 
   - 560de8e3a6c7 Pull complete                                          36.3s 
   - 0748cd1d792c Pull complete                                          36.3s 
[+] Building 3.3s (8/8) FINISHED
 => [internal] load build definition from Dockerfile                      0.0s
 => => transferring dockerfile: 148B                                      0.0s 
 => [internal] load .dockerignore                                         0.0s
 => => transferring context: 2B                                           0.0s 
 => [internal] load metadata for docker.io/library/node:18                2.6s 
 => [1/3] FROM docker.io/library/node:18@sha256:586cdef48f920dea2f47a954  0.0s
 => [internal] load build context                                         0.2s 
 => => transferring context: 15.31MB                                      0.2s 
 => CACHED [2/3] RUN mkdir -p /home/app                                   0.0s
 => [3/3] COPY . /home/app                                                0.2s 
 => exporting to image                                                    0.1s
 => => exporting layers                                                   0.1s 
 => => writing image sha256:d6e75178f06d60afda1b2aa446e1e81cd7a17ada25ac  0.0s
 => => naming to docker.io/library/mongoapp-curso-docker-main-app_ppy     0.0s 
[+] Running 3/2
 - Network mongoapp-curso-docker-main_default        Created              0.7s 
 - Container mongoapp-curso-docker-main-mongo_ppy-1  Created
```

El snippet anterior es solo una porción de la salida del comando, pero ahi se puede ver como se descargan las imágenes necesarias, crea los contenedores y arranca la aplicacion.

Al hacer esto, podemos simplemente volver a probar la aplicación en `localhost:3000`, y verificar que funcione.

{: .concept}
Con Docker Compose, lo único que hay que hacer para desplegar una aplicación es clonar el repositorio, y ubicados en la carpeta correcta ejecutar el comando `docker compose up`, y la aplicación ya estará corriendo.


{: .important}
Si nos fijamos en la terminal mientras la aplicación esta corriendo, vamos a ver que esta mostrando los logs. Para cerrar esto, y parar la aplicación, podemos hacer `ctrl+c`.

Habiendo detenido la aplicación, podemos ejecutar el comando `docker compose down` para eliminar todos los contenedores creados.

# Volúmenes

Durante este tutorial, estuvimos trabajando con una base de datos no relacional y creamos registros nuevos para probar la aplicación. Pero te habrás dado cuenta de que cada vez que eliminamos los contenedores y los volvimos a crear, la base de datos siempre estaba vacía.

Para lograr persistir estos datos, vamos a usar una herramienta más.

Los volúmenes en Docker son una forma de persistir los datos que se generan o se utilizan dentro de los contenedores, de forma que los datos no se pierdan al eliminarlos. Existen tres tipos de volúmenes:

- Volúmenes de host: permiten que un directorio del host sea montado dentro del contenedor.
- Volúmenes nombrados: son volúmenes independientes del host, que pueden ser utilizados por múltiples contenedores.
- Volúmenes de tipo tmpfs: se almacenan en la memoria RAM y se borran cuando se eliminan los contenedores que los utilizan.

También existen los volúmenes anónimos, que son temporales y se crean cuando se inicia un contenedor, pero se eliminan cuando este se detiene. Por otro lado, los volúmenes de tipo tmpfs se almacenan en la memoria RAM y se borran cuando se eliminan los contenedores que los utilizan.

Para empezar a usar volúmenes, volvamos al archivo docker-compose.yml.

A continuación de los servicios, vamos a especificar los volúmenes:

```yaml
volumes:
  mongo-data:
```

Con esta sección, estamos definiendo todos los volúmenes que se van a usar en nuestra aplicación.

Ahora tenemos que indicar qué volumen va a utilizar nuestra base de datos:

```yaml
mongo_ppy:
    image: mongo
    ports:
      - "27017:27017"
    environment:
      - MONGO_INITDB_ROOT_USERNAME=ppy
      - MONGO_INITDB_ROOT_PASSWORD=password
    volumes:
      - mongo-data: /data/db
```

La última sección indica cuál es el volumen que se va a utilizar, y hay que indicar también la ruta en la que se guardan los datos. En el caso de MongoDB, es `/data/db`.

Guardamos los cambios, nos aseguramos de que los contenedores estén abajo (con `docker compose down`), y podemos volver a crear los contenedores con `docker compose up`.

Una vez hecho esto, podemos volver a probar la aplicación, y nos vamos a dar cuenta de que podemos eliminar los contenedores y volver a crearlos cuantas veces queramos, y los datos van a persistir.