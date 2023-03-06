---
title: Bases de datos No Relacionales
has_children: false
parent: Introducción a NoSQL
nav_order: 1
has_toc: true
---
# Introducción a bases de datos no relacionales.
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Bases de datos relacionales

Las bases de datos relacionales surgieron en la década de 1970 como una forma más eficiente de almacenar y acceder a grandes cantidades de datos. Antes de su creación, los datos se almacenaban en archivos planos, lo que hacía que la gestión y el acceso a los datos fueran muy difíciles y propensos a errores. Las bases de datos relacionales resolvieron este problema al utilizar tablas con relaciones entre ellas para almacenar y acceder a los datos de manera más eficiente. Además, las bases de datos relacionales ofrecen una mayor consistencia y estructura en los datos, lo que las hace útiles para aplicaciones que requieren una gestión precisa de los datos.

Otra gran ventaja que nos otorgan las bases de datos relacionales es que son ACID compliant.

ACID es un acrónimo que se utiliza para describir las propiedades que se consideran importantes para una transacción en una base de datos. Las letras representan Atomicidad, Consistencia, Aislamiento y Durabilidad.

- **Atomicidad**: garantiza que una transacción se realiza como una operación indivisible, lo que significa que si una parte de la transacción falla, toda la transacción falla y se revierte a su estado anterior.
- **Consistencia**: garantiza que una transacción lleva la base de datos de un estado válido a otro estado válido. Esto significa que la base de datos debe cumplir con todas las restricciones de integridad y las reglas de negocio.
- **Aislamiento**: garantiza que una transacción se ejecuta de forma aislada de otras transacciones. Esto significa que el resultado de una transacción no se ve afectado por otras transacciones que se están ejecutando simultáneamente.
- **Durabilidad**: garantiza que los cambios realizados por una transacción se mantengan incluso en caso de un fallo del sistema o un reinicio.

En resumen, ACID es un conjunto de características que garantizan que las transacciones en una base de datos sean confiables, seguras y consistentes.

### Desventajas y limitaciones

Sin embargo, a medida que las aplicaciones se fueron volviendo más complejas y cambiaba la cantidad y el tipo de datos que necesitábamos almacenar, este tipo de bases de datos presentaba algunos problemas:

- **Dificultad para manejar datos no estructurados**: las bases de datos relacionales están diseñadas para manejar **datos estructurados** en tablas con relaciones entre ellas. Si se tienen datos no estructurados, como imágenes o texto no estructurado, puede ser difícil almacenarlos y procesarlos eficientemente.

{: .Note}
Pensemos en aplicaciones como las redes sociales que manejan una gran cantidad de datos no estructurados, como publicaciones, imágenes y vídeos.


- **Dificultad para escalar horizontalmente**: las bases de datos relacionales pueden tener dificultades para escalar horizontalmente a medida que aumenta la cantidad de datos y usuarios. Agregar más hardware puede ser costoso y difícil de administrar. Pensemos de nuevo en las redes sociales, que manejan enormes cantidades de datos.

{: .Note}
Estas bases de datos deben mantener relaciones entre las tablas, estas relaciones son difíciles de mantener si espaciamos estos datos en diferentes servidores.

- **Estructura rígida**: las bases de datos relacionales tienen una estructura fija y predefinida para los datos, lo que significa que cualquier cambio en la estructura de la base de datos puede ser complicado y requerir mucho tiempo.
- **Complejidad de consultas**: a medida que aumenta la complejidad de las consultas que se realizan en una base de datos relacional, puede ser más difícil para el motor de la base de datos procesarlas de manera eficiente.

## MongoDB y bases de datos no relacionales

Con la motivación de resolver los problemas de escalabilidad y flexibilidad nace MongoDB, que es una base de datos NoSQL que fue lanzada en 2009 por la compañía 10gen. Fue diseñada para manejar **grandes volúmenes de datos no estructurados** y **para escalar horizontalmente** de manera eficiente. MongoDB está escrito en C++ y es de código abierto. Desde su lanzamiento, ha ganado una gran popularidad entre desarrolladores y empresas que necesitan una base de datos flexible y escalable.

{: .Note}
El nombre mongo viene de la palabra informal hu**mongo**us, que significa enorme, y de la frase “Can store humongous amounts of data” (puede almacenar gigantescas cantidades de datos).

MongoDB, es una base de datos no relacional, o NoSQL (not only sql).

Una base de datos no relacional es una base de datos que no utiliza un modelo relacional para almacenar los datos. El lugar de utilizar tablas y relaciones para almacenar y acceder a los datos, las bases de datos no relacionales utilizan otros métodos, como documentos, columnas, gráficos, etc. Este modelo es útil para aplicaciones que requieren una gestión eficiente de grandes cantidades de datos no estructurados y una alta escalabilidad. Además,  son más flexibles que las bases de datos relacionales, ya que no tienen un esquema fijo y predefinido para los datos.

{: .Note}
Cuando decimos que no tienen un esquema fijo, nos referimos a que la base de datos no restringe la estructura o el tipo de datos que podemos guardar. Vamos a entender mejor este punto con la practica.

A pesar de que su popularidad es relativamente reciente, las bases de datos NoSQL son mas antiguas que las relacionales, que de hecho surgen como una mejora sobre los sistemas no relacionales existentes. Pero recientemente la tecnología y nuevos paradigmas han hecho que las NoSQL se vuelvan extremadamente eficientes y flexibles a un costo muy bajo.

### Bases de datos de documentos

Una base de datos de documentos es un tipo de base de datos no relacional que almacena y accede a los datos utilizando documentos en lugar de tablas con relaciones entre ellas. Cada documento contiene toda la información necesaria para un objeto en particular, y puede tener una estructura diferente sin estar obligado a seguir un esquema fijo. Los documentos pueden ser indexados y consultados de manera eficiente, lo que permite una alta escalabilidad y flexibilidad en la gestión de datos no estructurados.

![Ejemplos de documentos en una base de datos. Ver como cada documento tiene una estructura distinta.](images\documentos.png)

Ejemplos de documentos en una base de datos. Ver como cada documento tiene una estructura distinta.

Estos documentos están escritos en formato "JSON".

En una base de datos de documentos, no hay columnas como las que se encuentran en una base de datos relacional. En su lugar, los documentos contienen información estructurada en forma de pares clave-valor. Cada documento puede tener un conjunto diferente de claves y valores, lo que permite una mayor flexibilidad en la estructura de los datos.

{: .concept}
Sin embargo, podemos decir que una base de datos de documentos tiene solo dos columnas: un ID único y una columna que contiene el documento.

MongoDB es una base de datos de documentos.

![Untitled](images\relationalvsmongo.png)

### Características y ventajas de MongoDB

Además de la mayoría de las características generales de las bases de datos NoSQL, MongoDB tiene algunas características muy importantes y útiles:

**1.** MongoDB proporciona **alto rendimiento**. Las operaciones de entrada/salida son menores que en las bases de datos relacionales debido al soporte de Documentos integrados (modelos de datos) y las consultas Select también son más rápidas.

**2.** MongoDB tiene un **lenguaje de consulta rico** que admite todas las principales operaciones CRUD. El lenguaje de consulta también proporciona una buena búsqueda de texto y características de agregación.

**3.** La función de **replicación automática** de MongoDB conduce a una alta disponibilidad. Proporciona un mecanismo automático de conmutación por error, ya que los datos se restauran a través de copias de seguridad (réplicas) si el servidor falla.

**5.** MongoDB proporciona alta disponibilidad. La facilidad de replicación de MongoDB, llamada conjunto de réplicas, proporciona:

- Conmutación por error automática
- Redundancia de datos.

**6.** MongoDB proporciona escalabilidad horizontal como parte de su funcionalidad *principal*.

## Caso de estudio - Toyota

Toyota utiliza MongoDB como su base de datos principal para el software que utilizan en el sistema de automatización de depósitos.

Los requisitos originales eran:

- Rendimiento.
- Escalabilidad automática y servicio administrado.
- Seguridad y cumplimiento de normas internacionales.
- Copias de seguridad y restauración automáticas.
- Independencia de la nube.
- Fácil de usar.

El volumen de datos que generan y sus necesidades de análisis fueron clave para elegir MongoDB.

Pueden ver el caso de estudio completo en esta página: [https://www.mongodb.com/blog/post/video-toyota-industry-40-creating-smart-factory-moving-from-monolithic-code-base-microservices-mongodb-atlas-microsoft-azure](https://www.mongodb.com/blog/post/video-toyota-industry-40-creating-smart-factory-moving-from-monolithic-code-base-microservices-mongodb-atlas-microsoft-azure)

# Conceptos básicos

## Schema

En una base de datos NoSQL, el término "esquema" se refiere a la estructura de los datos almacenados. A diferencia de una base de datos relacional, una base de datos NoSQL no tiene una estructura fija y predefinida.

En su lugar, los datos se almacenan en documentos, columnas, gráficos o algún otro formato no tabular, y cada documento puede tener una estructura diferente. Esto permite una mayor flexibilidad y escalabilidad en la gestión de datos, pero también significa que no hay un esquema fijo para los datos.

Un problema de no usar un esquema fijo es que las aplicaciones que se conecten a la BD pueden guardar cualquier tipo de información, por lo que es más fácil cometer errores y tener problemas de consistencia.

Teniendo esto en cuenta, MongoDB ofrece validación de esquemas. Esto permite definir esquemas flexibles pero con reglas específicas que los documentos deben cumplir.

Con esta tecnología, MongoDB nos ofrece lo mejor de dos mundos: la flexibilidad, eficiencia y escalabilidad de NoSQL con la consistencia de datos de las BD relacionales.

## Model

En una base de datos, un modelo se refiere a la estructura de los datos almacenados y a la forma en que se relacionan entre sí.

En una base de datos relacional, el modelo se basa en tablas con relaciones entre ellas.

En una base de datos no relacional, como una base de datos de documentos, el modelo puede ser más flexible. El modelo es importante porque define cómo se almacenan y se accede a los datos, y puede afectar la eficiencia y la escalabilidad en el futuro.

## Json

JSON (JavaScript Object Notation) es un formato de archivo de datos que se utiliza para almacenar y transmitir datos estructurados. Es un formato de texto que es fácil de leer y escribir para los humanos, y fácil de analizar y generar para las máquinas. JSON se utiliza comúnmente en aplicaciones web y móviles para transmitir datos entre el servidor y el cliente.

Además, MongoDB almacena los datos en formato JSON.

```json
{
  "nombre": "Juan",
  "edad": 30,
  "hobbies": ["leer", "correr", "cocinar"],
  "direccion": {
    "calle": "Av. Siempreviva 123",
    "ciudad": "Springfield",
    "pais": "Estados Unidos"
  }
}

```

En este ejemplo, se tiene un objeto que contiene información sobre una persona. El objeto tiene cuatro propiedades: "nombre", "edad", "hobbies" y "dirección". La propiedad "nombre" es una cadena de caracteres, la propiedad "edad" es un número entero, la propiedad "hobbies" es un arreglo de cadenas de caracteres y la propiedad "dirección" es un objeto que contiene tres propiedades adicionales: "calle", "ciudad" y "país".

## Colección

Una colección en MongoDB es el equivalente a una tabla en una base de datos relacional. Es un conjunto de documentos almacenados dentro de una base de datos de MongoDB. Cada documento dentro de una colección puede tener una estructura diferente y no está obligado a seguir un esquema fijo, lo que permite una mayor flexibilidad y escalabilidad en la gestión de datos.

![Untitled](images\coleccion.png)

# Instalación de Mongo DB y operaciones básicas

### Instalando MongoDB

MongoDB tiene una amplia variedad de servicios y productos. Lo que nos interesa ahora es descargar el "Community Server".

Para ello, hay que navegar a la página web de MongoDB: [https://www.mongodb.com/](https://www.mongodb.com/).

A continuación, hay que ir a la pestaña de productos y seleccionar Community Server.

![step1.gif](images\download.gif)

Seleccionamos el sistema operativo que usamos y descargamos el instalador.

Una vez descargado, seguimos los pasos e instalamos normalmente.

Cuando termine la instalación, debería abrirse una ventana como esta:

![Untitled](images\compass.png)

MongoDB Compass es una herramienta gráfica para MongoDB que permite explorar y visualizar los datos almacenados.

Con Compass, los usuarios pueden ver la estructura de la base de datos, ejecutar consultas y visualizar los resultados en una interfaz gráfica fácil de usar. También permite realizar tareas de administración de la base de datos, como la creación de índices, la gestión de usuarios y la configuración de la replicación.

Al finalizar la instalación, podemos correr el comando `mongod --version` para comprobar si todo funciona.

Si tenemos Windows, es probable que esto no funcione porque no se encuentra el comando.

Para resolver esto, tenemos que agregar la ruta en la que se instaló MongoDB a la variable de entorno PATH:

![path.gif](images\path.gif)

Al hacer esto, tenemos que reiniciar cualquier instancia de la terminal que tengamos abierta para que se apliquen los cambios.

Una vez hecho todo, podemos volver a probar el comando en la terminal:

```powershell
PS C:\Users\fersz> mongod --version
db version v6.0.4
Build Info: {
    "version": "6.0.4",
    "gitVersion": "44ff59461c1353638a71e710f385a566bcd2f547",
    "modules": [],
    "allocator": "tcmalloc",
    "environment": {
        "distmod": "windows",
        "distarch": "x86_64",
        "target_arch": "x86_64"
    }
}
```

Ahora que tenemos esto resuelto, necesitamos iniciar la base de datos, pero MongoDB necesita un lugar donde guardar todas las bases de datos que creemos.

Para esto, tenemos que ir a nuestro disco duro y crear el directorio `data/db`.

```powershell
PS C:\> mkdir data/db

    Directory: C:\data

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----          3/5/2023   7:06 PM                db
```

con esto, podemos usar el comando `mongod` para iniciar MongoDB.

```powershell
PS C:\Users\fersz> mongod
{"t":{"$date":"2023-03-05T19:08:06.367-03:00"},"s":"I",  "c":"NETWORK",  "id":4915701, "ctx":"-","msg":"Initialized wire specification","attr":{"spec":{"incomingExternalClient":{"minWireVersion":0,"maxWireVersion":17},"incomingInternalClient":{"minWireVersion":0,"maxWireVersion":17},"outgoing":{"minWireVersion":6,"maxWireVersion":17},"isInternalClient":true}}}
{"t":{"$date":"2023-03-05T19:08:06.369-03:00"},"s":"I",  "c":"CONTROL",  "id":23285,   "ctx":"thread1","msg":"Automatically disabling TLS 1.0, to force-enable TLS 1.0 specify --sslDisabledProtocols 'none'"}
{"t":{"$date":"2023-03-05T19:08:07.285-03:00"},"s":"I",  "c":"NETWORK",  "id":4648602, "ctx":"thread1","msg":"Implicit TCP FastOpen in use."}
...
{"t":{"$date":"2023-03-05T19:08:07.591-03:00"},"s":"I",  "c":"INDEX",    "id":20345,   "ctx":"LogicalSessionCacheRefresh","msg":"Index build: done building","attr":{"buildUUID":null,"collectionUUID":{"uuid":{"$uuid":"53dd39e5-9ee4-4166-8a9a-1aa36eb37c7e"}},"namespace":"config.system.sessions","index":"_id_","ident":"index-5-8191881456430228189","collectionIdent":"collection-4-8191881456430228189","commitTimestamp":null}}
{"t":{"$date":"2023-03-05T19:08:07.592-03:00"},"s":"I",  "c":"INDEX",    "id":20345,   "ctx":"LogicalSessionCacheRefresh","msg":"Index build: done building","attr":{"buildUUID":null,"collectionUUID":{"uuid":{"$uuid":"53dd39e5-9ee4-4166-8a9a-1aa36eb37c7e"}},"namespace":"config.system.sessions","index":"lsidTTLIndex","ident":"index-6-8191881456430228189","collectionIdent":"collection-4-8191881456430228189","commitTimestamp":null}}
```

{: .important}
Es importante asegurarse de que el servicio haya quedado corriendo. Si en la terminal vemos el prompt (`PS C:\Users\fersz>`) listo para escribir otro comando, es que el servicio no se incicio.


Podemos detener el servicio con `ctrl + c`.

Debemos dejar esta ventana de la terminal abierta.

### Instalando MongoSH

Una vez que tenemos MongoDB instalado, necesitamos instalar MongoSH, que es la herramienta para gestionar la base de datos desde la línea de comandos. Podemos encontrar el instalador aquí: [https://www.mongodb.com/try/download/shell](https://www.mongodb.com/try/download/shell).

En la opción de sistema operativo, seleccionar ***`Windows 64-bit (8.1+) (MSI)`,*** descargar el instalador e instalar normalmente.

{: .important}
Es importante tomar nota de la ubicación donde instalamos para poder agregar esa ruta al PATH.

Con ambas herramientas, podemos comprobar la instalación haciendo `mongosh --version` en una nueva instancia de la terminal (no cerrar la que está corriendo el servicio de MongoDB).

```powershell
PS C:\Users\fersz> mongosh --version
1.8.0
```

{: .note}
Es posible que necesites agregar este comando al PATH, de la misma forma que en el caso anterior.

Si todo sale bien, podemos usar el comando `mongosh` para iniciar el SHELL:

```powershell
PS C:\Users\fersz> mongosh
Current Mongosh Log ID: 64051373f45ad036018cb3d8
Connecting to:          mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+1.8.0
Using MongoDB:          6.0.4
Using Mongosh:          1.8.0

For mongosh info see: https://docs.mongodb.com/mongodb-shell/

------
   The server generated these startup warnings when booting
   2023-03-05T19:10:55.813-03:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
   2023-03-05T19:10:55.814-03:00: This server is bound to localhost. Remote systems will be unable to connect to this server. Start the server with --bind_ip <address> to specify which IP addresses it should serve responses from, or with --bind_ip_all to bind to all interfaces. If this behavior is desired, start the server with --bind_ip 127.0.0.1 to disable this warning
------

------
   Enable MongoDB's free cloud-based monitoring service, which will then receive and display
   metrics about your deployment (disk utilization, CPU, operation statistics, etc).

   The monitoring data will be available on a MongoDB website with a unique URL accessible to you
   and anyone you share the URL with. MongoDB may use this information to make product
   improvements and to suggest MongoDB products and deployment options to you.

   To enable free monitoring, run the following command: db.enableFreeMonitoring()
   To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
------

test>
```

Si nos fijamos en el último prompt, `test>`, vemos que nos encontramos "dentro" del servidor de MongoDB. `test` es el nombre de la base de datos que estamos usando. Es una base de datos que se crea por defecto, pero que en realidad no existe hasta que le carguemos datos.

Para ver todas las bases de datos que existen, podemos usar el comando `show dbs`:

```powershell
test> show dbs
admin   40.00 KiB
config  12.00 KiB
local   72.00 KiB
test>
```

Otros comandos que podemos utilizar:

- `use`: se utiliza para seleccionar y utilizar una base de datos. Actualmente estamos en `test`, pero podemos cambiar a la base de datos `admin` con el comando: `use admin`.
- `db.dropDatabase()`: Elimina una base de datos completa.

Para los siguientes ejemplos, vamos a utilizar una nueva base de datos de prueba:

Ejecutemos el comando `use appdb` para crear una base de datos llamada appdb.

```powershell
test> use appdb
switched to db appdb
appdb>
```

Con esto, estamos listos para explorar el resto de las interacciones con la base de datos, empezando por aprender a leer y escribir.