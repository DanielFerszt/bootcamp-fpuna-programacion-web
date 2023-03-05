---
title: Hot Reload y entornos diferentes
has_children: false
parent: Introducción a Docker
nav_order: 3
has_toc: true
---
# Configurando diferentes ambientes
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>


Existen muchas herramientas que nos pueden resultar útiles durante el desarrollo, pero que no queremos instalarlas cuando se hace el despliegue en otros entornos como testing o producción.

Docker nos permite gestionar estos ambientes distintos usando diferentes versiones del archivo `dockerfile`.

Para probar esto, lo primero que necesitamos hacer es crear un archivo nuevo en la raíz de nuestro proyecto, llamado `dockerfile.dev`.

El archivo empieza igual que el Dockerfile original:

```yaml
FROM node:18
```

Pero después de crear la imagen de Node, vamos a instalar una nueva dependencia. `nodemon` se encarga de reiniciar automáticamente el servidor de Node cuando cambia algún archivo en el directorio. Con esto, vamos a evitar tener que apagar todos los contenedores y volver a arrancarlos cada vez que queremos hacer un cambio:

```yaml
RUN npm -i -g nodemon
RUN mkdir -p /home/app
```

El siguiente comando es el mismo que teníamos en el archivo original, y a continuación vamos a definir un directorio en el cual se va a trabajar siempre:

```yaml
WORKDIR /home/app
```

Esto le indica a Docker que cualquier instrucción que le demos se debe ejecutar desde esa ubicación, por lo que podemos cambiar la forma en que levantamos la aplicación, ejecutando directamente el comando `node index.js`.

También podemos eliminar el comando `COPY` que usábamos para copiar el código fuente, ya que el volumen que configuramos se encargará de copiar todos los datos.

El archivo completo debería quedar así:

```yaml
FROM node:18

RUN npm -i -g nodemon
RUN mkdir -p /home/app

WORKDIR /home/app

EXPOSE 3000

CMD ["nodemon", "index.js"]
```

Ahora tenemos que crear un nuevo archivo para Docker Compose. Esta vez se debe llamar `docker-compose-dev.yml`. Copiamos todo el contenido del archivo original y hacemos algunos cambios.

El código completo debe quedar así:

```yaml
version: "3.9"
services:
  app_ppy:
    build: 
      context: .
      dockerfile: dockerfile.dev
    ports:
      - "3000:3000"
    links:
      - mongo_ppy
    volumes:
      - .:/home/app
  mongo_ppy:
    image: mongo
    ports:
      - "27017:27017"
    environment:
      - MONGO_INITDB_ROOT_USERNAME=ppy
      - MONGO_INITDB_ROOT_PASSWORD=password
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
```

Lo que hemos cambiado en este archivo son dos cosas:

- En primer lugar, hemos modificado la sección `build` de `app_ppy` para agregarle dos nuevas propiedades:
    - `context` indica dónde se encuentra el código fuente.
    - `dockerfile` indica cuál es el archivo Dockerfile que vamos a utilizar.
- También hemos agregado una sección `volumes` dentro del servicio `app_py`, donde declaramos un volumen anónimo y le indicamos su ubicación. Este volumen contendrá el código fuente.

Con estos cambios, podemos volver a ejecutar Docker Compose, pero debemos indicar que queremos utilizar el nuevo archivo.

Realizamos lo siguiente:

```powershell
docker compose -f docker-compose-dev.yml up
```

Usamos la bandera `-f` para indicar que vamos a usar un archivo diferente.

Con esto, la aplicación debería estar funcionando, y si hacemos algún cambio en el archivo `index.js`, nodemon se encarga de reiniciar la app y deberíamos ver el cambio en vivo.

{: .important}
Si luego de hacer cambios en el codigo fuente no vemos que el servicio se reinicie, podemos editar el `dockerfile.dev`y cambiar la ultima linea para que quede asi: `CMD *[*"nodemon"*,* "-L"*,* "index.js"*]`.* Este es un problema conocido en Windows, que no permite que el contenedor disponga del sistema de archivos del anfitrion.

Con esto termina el tutorial y esperamos que puedas seguir aprendiendo sobre Docker por tu cuenta.