---
title: Recetario de Jugos - API
has_children: false
parent: Introducción a NoSQL
nav_order: 4
has_toc: true
---
# Recetario de Jugos - API
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

En esta sección vamos a desarrollar una API simple que se conecte a MongoDB, para permitir que el front end de nuestra aplicación de recetas se conecte con la base de datos.

![MERN](images\mern.png)

En la imagen de arriba podemos ver la arquitectura mas común para aplicaciones con el stack MERN (**M**ongoDB, **E**xpressJS, **R**eactJS, **N**odeJS), que es uno de los stacks mas populares.

En casos como estos, donde el sistema esta distribuido en distintos servicios, el backend se comunica con el front a traves de una API.

En este proyecto, vamos a trabajar en el backend.

---

# Recursos

- [Mongoose Crash Course - Beginner Through Advanced](https://www.youtube.com/watch?v=DZBGEVgL2eE&t=30s&ab_channel=WebDevSimplified)
- [Descarga de Cheat-Sheet](https://webdevsimplified.com/mongodb-cheat-sheet.html)
- [Mongo Playground](https://mongoplayground.net/)

# Setup

Vamos a seguir estos pasos para preparar el proyecto:

1. Creamos una nueva carpeta en el escritorio o en un directorio de nuestra elección, con el nombre que le queramos dar al proyecto.
2. Abrimos la carpeta con Visual Studio Code y creamos una terminal integrada (`ctr+shift+p`).
3. En la terminal, inicializamos un proyecto de NodeJS corriendo `npm init --yes`, siempre en la carpeta del proyecto.
Esto crea un archivo `package.json` con algunos datos de configuración.
4. Ahora, necesitamos un servidor web para nuestra aplicación. Vamos a usar un Framework llamado ExpressJS, que instalamos con `npm install express --save`, esto crea la carpeta node-modules donde se guardan todas las dependencias.

{: .concept}
ExpressJS es un framework de Node.js utilizado para construir aplicaciones web y APIs. Es muy popular porque es fácil de usar y es altamente personalizable.

{: .concept}
El flag `--save` en el comando `npm install` indica que la dependencia que estamos instalando debe ser añadida automáticamente al archivo `package.json`. Esto es útil para que cualquier otra persona que descargue nuestro proyecto pueda instalar todas las dependencias necesarias ejecutando simplemente el comando `npm install`.

# Preparando el Web Server

Ahora que tenemos el proyecto preparado, podemos crear el archivo `index.js` en la carpeta.

En este archivo, tenemos que requerir ExpressJS:

```jsx
const express = require('express');

const app = express();
```

Finalmente, podemos decirle al web server que espere en el puerto 3000 por cualquier request: `app.listen(3000, () => console.log("Esperando en puerto 3000..."))`

Si ejecutamos en la terminal `node index.js`, deberíamos ver que el puerto se inicia, pero nuestra app todavía no hace nada.

Antes de continuar, vamos a instalar Nodemon para que se nos facilite el desarrollo. 

Detenemos el server y corremos `npm i -g nodemon` en la terminal.

{: .concept}
El flag `-g` en el comando `npm install` indica que la dependencia que estamos instalando debe ser añadida globalmente a nuestro sistema, para que pueda ser utilizada por cualquier proyecto que lo requiera. Por ejemplo, si instalamos `nodemon` con el flag `-g`, podemos utilizarlo en cualquier proyecto de Node.js que tengamos en nuestra máquina sin tener que instalarlo de nuevo en cada proyecto.

Para correr el server, tenemos que usar `nodemon index.js`.

Ahora si, estamos listos para empezar a añadir rutas.

# Configurando rutas con ExpressJS

En ExpressJS, una ruta es una sección del código que maneja solicitudes HTTP específicas. 

Las rutas se definen mediante un URI (Uniform Resource Identifier), un método HTTP y una función de controlador que se llama cuando se coincide la ruta. 

El método HTTP puede ser GET, POST, PUT, DELETE, etc., y la función de controlador se ejecuta cuando se recibe una solicitud HTTP en esa ruta.

Hagamos un ejemplo, vamos a agregar una ruta para crear un usuario nuevo:

```javascript
app.post('/new_user', (req,res) => {
    res.send(req.body)
})
```

`app` hace referencia a nuestro web server, y lo que hacemos en este bloque es crear un método `POST.` 

{: .concept}
El método HTTP POST se utiliza para enviar datos a un servidor para crear o actualizar un recurso. Es uno de los métodos HTTP más comunes y se utiliza para enviar datos de un cliente a un servidor a través de una solicitud HTTP.


Al método post le pasamos la ruta correspondiente, en este caso `/new_user`, esto significa que cualquiera que acceda al web server, si navega a esta ruta, va a ejecutar la función anónima que esta como segundo argumento.

Se accede al servidor web a traves del dominio en el que se encuentra, en este caso `localhost`, y el puerto en el que esta escuchando, asi: `localhost:3000`, asi que la URI para crear un nuevo usuario seria `localhost:3000/new_user`.

El segundo argumento, la funcionan anónima `(req, res) ⇒ { … }` toma dos argumentos. El primero es el `REQUEST`, y nos sirve para manipular los datos que recibamos, y el segundo es el `RESPONSE`, que nos permite enviar una respuesta. Esto es necesario según el funcionamiento del protocolo REST.

{: .concept}
REST (Representational State Transfer) es una arquitectura de software que define un conjunto de restricciones para la creación de servicios web. Se basa en el protocolo HTTP y se utiliza para crear aplicaciones y servicios que son escalables y fáciles de mantener. Los servicios RESTful utilizan URLs para identificar los recursos y los métodos HTTP (como GET, POST, PUT y DELETE) para realizar operaciones en esos recursos.


En este bloque de código lo que hacemos es leer lo que sea que estamos enviando y volver a enviar los mismos datos, un simple `ECHO`, solo para probar que todo funcione.

Pero esto todavía no funciona, ya que los pedidos que nos vengan van a tener los datos en formato JSON, que nuestra app no sabe leer. Para resolver esto hacemos `app.use(express.json())` justo después de instanciar el web server.

Nos debería quedar todo asi:

```jsx
const express = require('express');

const app = express();

//Middleware
app.use(express.json())

// Routes
app.post('/new_user', (req,res) => {
    res.send(req.body)
})

app.listen(3000, () => console.log("Esperando en puerto 3000..."))
```

# Insomnia

Insomnia es una herramienta de software que se utiliza para probar y depurar APIs. Permite enviar solicitudes HTTP a una API, ver la respuesta y verificar que la API funciona correctamente. También tiene otras funciones útiles, como la capacidad de guardar solicitudes, organizar solicitudes en carpetas y compartir solicitudes con otros desarrolladores. 

Puedes instalar Insomnia siguiendo estos pasos:

1. Ve al sitio web oficial de Insomnia: [https://insomnia.rest/](https://insomnia.rest/)
2. Haz clic en "Download" en la parte superior de la página.
3. Selecciona la versión adecuada para tu sistema operativo y haz clic en el botón de descarga.
4. Una vez descargado, instala Insomnia siguiendo las instrucciones que aparecen en pantalla.

Con insomnia instalado, podemos probar nuestra api. Abrimos insomnia, seleccionamos la opción `POST` y en la URI ponemos `[localhost:3000/new_user](http://localhost:3000/new_user)`.

Seleccionamos “JSON” como payload, y ponemos este dato:

```json
{
"name": "Felix"
}
```

Hacemos click en SEND, y deberíamos ver que la app funciona.

Podemos crear las demás rutas que vamos a necesitar, y el código queda asi:

```javascript
const express = require('express');

const app = express();

//Middleware
app.use(express.json())

// Routes
app.post('/new_user', (req,res) => {
    res.send(req.body)
})

app.post('/new_recipe', (req,res) => {
    res.send(req.body)
})

app.post('/rate', (req,res) => {
    res.send(req.body)
})

app.get('/recipes', (req,res) => {
    res.send("Enviamos las recetas")
})

app.listen(3000, () => console.log("Esperando en puerto 3000..."))
```

A continuación, podemos ir trabajando en cada una de las rutas.

# Data Modeling con Mongoose

Mongoose es una librería de JavaScript que proporciona una solución basada en esquemas para modelar datos de MongoDB. 

Permite definir objetos con un esquema fuertemente tipado que se mapea a documentos en una base de datos MongoDB. 

Además, ofrece muchas funciones útiles para trabajar con MongoDB, como validación de esquemas, consultas avanzadas y middleware.

Vamos ir viendo algunas cosas básicas de Mongoose, pero lo primero sera configurar todo.

Instalamos Mongoose con el comando `npm i mongoose --save`.

Mientras tanto, podemos importar la librería en el archivo `index.js` con `const *mongoose* = require('mongoose');` justo debajo de donde importamos ExpressJS.

Ahora necesitamos conectarnos a una base de datos.

### Actividad - Levantando una instancia de Mongo con Docker

Para utilizar MongoDB en este proyecto y cementar conocimientos adquiridos en otros proyectos, vamos a levantar una instancia de Mongo con docker.

Ya deberías saber como hacer esto, pero aca van algunos tips:

- Asegurate de que no haya ninguna instancia de Mongo corriendo para evitar conflictos.
- Podes usar el comando `Docker run`.
- Vas a tener que mapear puertos.
- El nombre del contenedor debería estar relacionado a la app que estamos haciendo.
- Sera necesario conectarse con nombre de usuario y contraseña.
- Usa el tutorial de docker como referencia para hacer esto.

Mas adelante también vas a tener que dockerizar la app completa, incluyendo una instancia de Mongo (como hicimos en el tutorial de docker).

Para conectarnos a mongoDB, usamos este método en `index.js`:

```javascript
// MongoDB connection
mongoose.connect('mongodb://poli:password@localhost:27017/miapp?authSource=admin')
.then(() => console.log('conectado a Mongo'))
.catch((error) => console.error(error))
```

El método `connect()` es asíncrono, eso significa que en realidad devuelve una [promesa](https://developer.mozilla.org/es/docs/Web/JavaScript/Guide/Using_promises). Esto significa que podemos encadenar el metodo `then()`, que se ejecuta cuando la operación tiene éxito, y el método `catch()`, que atrapa cualquier excepción que se pueda dat.

## User Schema

Ahora que estamos conectados a la base de datos podemos empezar con el modelado.

Primeramente vamos a crear una carpeta en el directorio del proyecto, llamada `models`. Ahi vamos a guardar todos nuestros modelos de datos.

De acuerdo al modelado que hicimos en la sección anterior, tenemos dos colecciones: users y recipes.

Vamos a empezar por modelar la entidad `user`.

Primero creamos un archivo `user.js` dentro de `models`

En el archivo nuevo, podemos definir un SCHEMA para esta entidad, usando `mongoose.Schema`: 

```javascript
const mongoose = require('mongoose')

const userSchema = mongoose.Schema({
    name: {
        type: String,
        required: true,
        lowercase: true
    },
    schema: {
        type: Number,
        required: true
    }
})

module.exports = mongoose.model('user', userSchema)
```

En la primera linea importamos mongoose como siempre. Seguidamente, vamos a definir un esquema donde tenemos dos campos `name` y `schema`, para cada uno declaramos tipos y validaciones.

Si tratamos de guardar algún valor que no cumpla estas validaciones, la operación va arrojar un error.

Esto nos permite ser tan estrictos como queramos con los datos que guardamos en la base de datos, ayudándonos a mantener un nivel de consistencia apropiado.

## Endpoint para crear usuario

Ya tenemos el modelo, ahora tenemos que completar el endpoint.

Lo primero es importar el modelo creado en `index.js` con `const userSchema = require('./models/user')`.

En la ruta correspondiente, podemos guardar los datos obtenidos con el método `save()` de mongoose:

```javascript
app.post('/new_user', (req,res) => {
    const user = userSchema(req.body);
    user.save()
    .then((data) => res.json(data))
    .catch((error) => res.send(error))
})
```

Corroboramos que el endpoint funcione con insomnia. Podemos usar la misma petición que usamos antes:

![insomnia](images\insomnia_save_user.png)

Nuestro endpoint de vuelve lo que MongoDB retorna si la operación tiene éxito, y si no, el mensaje de error correspondiente.

También podemos ver con MongoDB Compass o en la terminal que los datos efectivamente se están guardando en la BD.

![compass](images\compass_save_user.png)

## Recepie Schema

La entidad para las recetas es mas compleja, y vamos a tener que hacer algo nuevo.

Recordemos que en el esquema que desarrollamos, en realidad teníamos 4 entidades:

- User
- Recepie
- Ingredient
- Rating

Pero tanto `Rating` como `Ingredient` estaban “embebidos” en `Recepie`. Vamos a tener que replicar esto en el esquema de mongoose.

Para hacer esto, seguimos los mismos pasos que con la entidad `user`, pero en el archivo `recipe.js`.

En lugar del esquema para los usuarios, primero vamos a modelar la entidad `ingredient`:

```javascript
const ingredientSchema = mongoose.Schema({
    name: {
        type: String,
        required: true,
        lowercase: true
    },
    qty: {
        type: Number,
        required: true
    },
    unit: {
        type: String,
        required: true,
        lowercase: true
    }
}, { _id: false})
```

El siguiente paso es modelar `rating`:

```javascript
const ratingSchema = mongoose.Schema({
    userId: {
        type: ObjectId,
        required: true
    },
    rating: {
        type: Number,
        required: true,
        validate: {
            validator: v => v >= 0 && v<=5,
            message: "rating debe estar entre 0 y 5"
        }
    },
}, { _id: false})
```

y finalmente podemos modelar la entidad `recipe`:

```jsx
const recipeSchema = mongoose.Schema({
    schema: {
        type: Number,
        required: true
    },
    userId: {
        type: ObjectId,
        required: true
    },
    name: {
        type: String,
        required: true,
        lowercase: true,
    },
    ingredients: {
        type: [ingredientSchema],
        validate: {
            validator: v => v.length > 0,
            message: "Sin ingredientes"
        }
    },
    method: {
        type: String,
        required: true,
        lowercase: true,
    },
    rating: [ratingSchema],
    avgRating: Number
})
```

Hay muchas cosas interesantes en este esquema!

### Subdocuments.

En Mongoose, los subdocumentos son documentos anidados dentro de otro documento que tienen su propio esquema. Son útiles para modelar relaciones de uno a muchos, donde un documento principal tiene varios subdocumentos asociados.

En el esquema de la entidad `recipe`, `ratingSchema` y `ingredientSchema` son ejemplos de subdocumentos porque están anidados dentro del esquema principal de `recipeSchema`.

Al definir un subdocumento, es importante especificar que `_id` sea `false` en el segundo argumento del constructor `mongoose.Schema()`, para que no se cree un identificador único para cada subdocumento. En lugar de eso, el `_id` del subdocumento se deriva del documento principal.

Para acceder a un subdocumento en el código, se utiliza la notación de punto, por ejemplo `recipe.ingredients[0].name` accedería al nombre del primer ingrediente en la receta.

### Validaciones

Mongoose ofrece la posibilidad de añadir validaciones a los esquemas de las entidades. En el ejemplo del documento, se pueden ver algunas validaciones aplicadas a los campos de las entidades `user`, `ingredient`, `rating` y `recipe`. 

Por ejemplo, en el esquema de `user`, se establece que el campo `name` es obligatorio (`required: true`) y debe estar en minúsculas (`lowercase: true`). 

En el esquema de `ingredient`, se establece que todos los campos son obligatorios (`required: true`), y que el campo `unit` también debe estar en minúsculas. En el esquema de `rating`, se establece una validación personalizada para el campo `rating`, que debe estar entre 0 y 5 (`validator: v => v >= 0 && v<=5`). 

En el esquema de `recipe`, se establece una validación personalizada para el campo `ingredients`, que debe tener al menos un ingrediente (`validator: v => v.length > 0`).

### Actividad - Endpoint para registrar receta

Habiendo creado el endpoint para registrar un usuario, esto debería ser fácil!

Termina el endpoint para registrar una receta con el modelo que creamos anteriormente, en la ruta `/new_recipe`.

Proba la api con Insomnia para saber que esta bien.

### Actividad - Endpoint para obtener todas las recetas de un usuario

Ahora que podemos registrar recetas, hagamos el endpoint para consultar todas las recetas de un usuario.

La ruta debería ser `/recipes`.

Como siempre, probamos la api con Insomnia.

## Endpoint para calificar recetas

Ahora hagamos algo mas complejo. Queremos hacer un update sobre un documento de la colección para registrar una calificación nueva.

Lo que tenemos que tener en cuenta, es que queremos agregar un objeto nuevo a un array.

Nuestro payload tendría esta forma:
```json
{
	"recipeId": "640b8aa76ae2562fb9023dcf",
	"userId": "640a46b8b9447ba3c4832c3d",
	"rating": 5
}
```

El objeto que tenemos que agregar al array `ratings` de nuestro documento, tiene esta forma:
```json
{
	"userId": "640a46b8b9447ba3c4832c3d",
	"rating": 5
}
```

Si solo queremos agregar la calificación, podemos hacer `db.updateOne({ _id: <id> }, {$push: {ratings: <objeto>}})`.

Sin embargo, ademas de hacer el push, queremos actualizar el campo `avgRating` con el nuevo promedio. Hay muchas formas de hacer esto, una de ellas es usando un **update con agregación**.

Esto tiene esta forma:
```jsx
db.updateOne(
    { _id: <id> },
    [
        { <primera etapa> },
        { <segunda etapa> }
    ]
)
```
Nos permite encadenar muchas operaciones en etapas distintas.
En la primera etapa podemos hacer el push, y en la segunda calculamos el promedio. El problema, es que no se puede usar el comando `$push` en un pipeline de agregacion como este.

En la sección anterior ya trabajamos en una solución para esto, nuestra ruta quedaría asi:
```js
app.put('/rate', (req,res) => {
    const { recipeId, userId , rating } = req.body
    recepieSchema
    .updateOne(
        { _id: recipeId },
        [
            { $set: { ratings: {$concatArrays: [{$ifNull: ['$ratings', []]}, [{ userId: userId, rating: rating}]]}} },
            {$set: {avgRating: {$trunc: [{$avg:['$ratings.rating']},0]}}}
        ]
    )
    .then((data) => res.json(data))
    .catch((error) => {
        console.log(error)
        res.send("error")
    })
})
```
# Tarea - API de Recetas.

Termina la API por tu cuenta, con estos requerimientos:

- El proyecto debe estar en un repositorio publico en git.
- Todo el proyecto debe estar dockerizado.
    - Para evaluar, se va a usar el comando `docker compose up` en el proyecto. Si algo no funciona al hacer esto, no se podrá evaluar el resto.
- Se van a evaluar los siguientes endpoints:
    - **Crear usuario:**
        - Ruta: `localhost:3000/new_user`
        - Payload:
        
        ```json
        {
        	"schema": 0,
        	"name": "Felix"
        }
        ```
        
    - **Crear Receta:**
        - Ruta: `localhost:3000/new_recipe`
        - Payload:
        
        ```json
        {
            "schema": 0,
            "userId": "640a46b8b9447ba3c4832c3d",
            "name": "jugo de banana",
            "ingredients": [
                {
                    "name": "banana",
                    "qty": 2,
                    "unit": "unit"
                },
                {
                    "name": "leche",
                    "qty": 300,
                    "unit": "ml"
                }
            ],
            "method": "instrucciones"
        }
        ```
        
    - **Recetas por ingredientes:**
        - Ruta: `localhost:3000/recipesbyingredient`
        - Payload:
        
        ```json
        {
        	"ingredients": [
        		{
        			"name": "banana"
        		},
        		{
        			"name": "leche"
        		}
        	]
        }
        ```
        
        - Respuesta esperada:
            - Solo las recetas que PUEDO hacer con los ingredientes que tengo.
                - Las especias de cualquier tipo, el agua y el hielo no se tienen en cuenta. No se deberían agregar a la lista de ingredientes de la receta.
                - Si tengo banana y leche, no puedo hacer una receta que tenga banana, leche, y durazno.
                - Si tengo banana y leche, SI puedo hacer una receta que tenga banana y agua.
                - Se traen todos los campos de la receta.
            
            {: .important}
            Es importante pensar en Edge Cases!
            
            
    - **Recetas por usuario:**
        - Ruta: `localhost:3000/recipes`
        - Payload:
        
        ```json
        {
            "userId": "640a46b8b9447ba3c4832c3d"
        }
        ```
        
        - Respuesta esperada:
            - Se traen todas las recetas que pertenezcan al usuario.
            - Se traen todos los campos de la receta.

## Rubrica

| Funcionalidad | Puntaje |
| --- | --- |
| Empaquetado con docker | 20 |
| Crear usuario | 20 |
| Crear Receta | 20 |
| Recetas por ingredientes | 20 |
| Recetas por usuario | 20 |
| Total | 100 |

{: .important}
Si el proyecto no esta en repositorio de git no sera evaluado.

{: .important}
Si el empaquetado de docker no funciona, no se podrán evaluar los siguientes puntos.
