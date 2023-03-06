---
title: Operaciones Básicas
has_children: false
parent: Introducción a NoSQL
nav_order: 2
has_toc: true
---
# Operaciones Básicas en MongoDB

{: .important}
Para desarrollar el siguiente contenido, es importante tener tanto MongoDB como MongoSH corriendo. Fijarse en seccion anterior.

## Insertar y leer datos

Podemos insertar datos con el comando `insertOne()`. Para el primer ejemplo, ejecutemos el comando `db.users.insertOne({name: "Juan"})`:

```powershell
appdb> db.users.insertOne({name: "Juan"})
{
  acknowledged: true,
  insertedId: ObjectId("640516cca8f69ebadb87ea41")
}
appdb>
```

Este comando hizo dos cosas.

Lo que le pedimos a MongoDB es que inserte un **documento** en la colección `users`.

- Como la colección `users` no existe, con este comando se crea.
- Una vez creada la colección, se inserta el documento.

`{name: "Juan"}` es el documento insertado.

Para acceder a los datos de la colección, podemos usar el método `find()`.

```powershell
appdb> db.users.find()
[ { _id: ObjectId("640516cca8f69ebadb87ea41"), name: 'Juan' } ]
appdb>
```

Esto nos devuelve un array con todos los documentos en la colección. En este caso hay uno solo.

Hasta aca podemos ver un par de cosas importantes sobre las bases de datos NoSQL.

1. No tuvimos que definir ninguna columna, ninguna tabla, ningún esquema. Solo insertamos un documento y funciono.
2. MongoDB agrego un ID único a nuestro documento. Esto se hace siempre de forma automática, y podemos usar ese ID mas adelante para consultar la base de datos desde una aplicación.

Como estos datos que insertamos son documentos, y no tienen un esquema, podemos hacer lo siguiente:

```powershell
appdb> db.users.insertOne({name: "Anna", edad: 20, direccion: {calle:"colon", numero:2285}, hobbies:["correr"]})
{
acknowledged: true,
insertedId: ObjectId("64051a61a8f69ebadb87ea42")
}
appdb>
```

y podemos consultar los datos:

```powershell
appdb> db.users.find()
[
  { _id: ObjectId("640516cca8f69ebadb87ea41"), name: 'Juan' },
  {
    _id: ObjectId("64051a61a8f69ebadb87ea42"),
    name: 'Anna',
    edad: 20,
    direccion: { calle: 'colon', numero: 2285 },
    hobbies: [ 'correr' ]
  }
]
appdb>
```

Vemos aca que pudimos insertar dos documentos con estructuras distintas, y que podemos anidar objetos.

También podemos usar el comando `insertMany`, que toma como argumento un array donde podemos listar varios objetos.

Usemos este comando para insertar varios documentos de una vez:

```json
[{name: "Pedro", edad: 26, hobbies: ["power lifting", "Football"], direccion: {calle: "Ayolas", numero:456, ciudad: "Asuncion"}},{name: "Guillermo", edad: 41, hobbies: ["Natacion"], direccion: {calle: "Lilio", numero:1245, ciudad: "Asuncion"}},{name: "Felix", edad: 32, hobbies: ["Ciclismo", "Senderismo"], direccion: {calle: "Piribebuy", numero:6521, ciudad: "Asuncion"}},]
```

```powershell
appdb> db.users. insertMany([{name: "Pedro", edad: 26, hobbies: ["power lifting", "Football"], direccion: {calle: "Ayolas", numero:456, ciudad: "Asuncion"}},{name: "Guillermo", edad: 41, hobbies: ["Natacion"], direccion: {calle: "Lilio", numero:1245, ciudad: "Asuncion"}},{name: "Felix", edad: 32, hobbies: ["Ciclismo", "Senderismo"], direccion: {calle: "Piribebuy", numero:6521, ciudad: "Asuncion"}}])
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId("6405216ea8f69ebadb87ea43"),
    '1': ObjectId("6405216ea8f69ebadb87ea44"),
    '2': ObjectId("6405216ea8f69ebadb87ea45")
  }
}
appdb>
```

podemos consultar la colección para ver que es lo que se creo:

```powershell
appdb> db.users.find()
[
  { _id: ObjectId("640516cca8f69ebadb87ea41"), name: 'Juan' },
  {
    _id: ObjectId("64051a61a8f69ebadb87ea42"),
    name: 'Anna',
    edad: 20,
    direccion: { calle: 'colon', numero: 2285 },
    hobbies: [ 'correr' ]
  },
  {
    _id: ObjectId("6405216ea8f69ebadb87ea43"),
    name: 'Pedro',
    edad: 26,
    hobbies: [ 'power lifting', 'Football' ],
    direccion: { calle: 'Ayolas', numero: 456, ciudad: 'Asuncion' }
  },
  {
    _id: ObjectId("6405216ea8f69ebadb87ea44"),
    name: 'Guillermo',
    edad: 41,
    hobbies: [ 'Natacion' ],
    direccion: { calle: 'Lilio', numero: 1245, ciudad: 'Asuncion' }
  },
  {
    _id: ObjectId("6405216ea8f69ebadb87ea45"),
    name: 'Felix',
    edad: 32,
    hobbies: [ 'Ciclismo', 'Senderismo' ],
    direccion: { calle: 'Piribebuy', numero: 6521, ciudad: 'Asuncion' }
  }
]
appdb>
```

## Consultas mas complejas

Podemos hacer consultas mas complejas usando el método find().

Por ejemplo, si hacemos `db.users.find().limit(2)`, podemos limitar la cantidad de resultados, en este caso a 2:

```powershell
appdb> db.users.find().limit(2)
[
  { _id: ObjectId("640516cca8f69ebadb87ea41"), name: 'Juan' },
  {
    _id: ObjectId("64051a61a8f69ebadb87ea42"),
    name: 'Anna',
    edad: 20,
    direccion: { calle: 'colon', numero: 2285 },
    hobbies: [ 'correr' ]
  }
]
appdb>
```

También podemos ordenar por cualquier campo:

```powershell
appdb> db.users.find().sort({name: 1}).limit(3)
[
  {
    _id: ObjectId("64051a61a8f69ebadb87ea42"),
    name: 'Anna',
    edad: 20,
    direccion: { calle: 'colon', numero: 2285 },
    hobbies: [ 'correr' ]
  },
  {
    _id: ObjectId("6405216ea8f69ebadb87ea45"),
    name: 'Felix',
    edad: 32,
    hobbies: [ 'Ciclismo', 'Senderismo' ],
    direccion: { calle: 'Piribebuy', numero: 6521, ciudad: 'Asuncion' }
  },
  {
    _id: ObjectId("6405216ea8f69ebadb87ea44"),
    name: 'Guillermo',
    edad: 41,
    hobbies: [ 'Natacion' ],
    direccion: { calle: 'Lilio', numero: 1245, ciudad: 'Asuncion' }
  }
]
appdb>
```

En este caso, estamos ordenando por el nombre, de forma ascendente, por eso el 1. Si como argumento podemos `{name: -1}`, se hace de forma descendente.

También podemos ordenar por varios campos haciendo algo como esto: `db.users.find().sort({name: 1, edad: 1}).limit(3)`.

Para hacer consultas equivalentes a un `SELECT ... WHERE` de SQL, podemos usar el comando `find()` pasando como argumento un objeto con las condiciones:

```powershell
appdb> db.users.find({name: "Felix"})
[
  {
    _id: ObjectId("6405216ea8f69ebadb87ea45"),
    name: 'Felix',
    edad: 32,
    hobbies: [ 'Ciclismo', 'Senderismo' ],
    direccion: { calle: 'Piribebuy', numero: 6521, ciudad: 'Asuncion' }
  }
]
appdb>
```

En este ejemplo, obtuvimos el documento donde el nombre es Felix, si habían varios, el comando nos trae todos.

También podemos hacer esto mismo pero pedir que Mongo nos devuelva solo un parámetro:

```powershell
appdb> db.users.find({name: "Felix"}, {name: 1, edad: 1})
[
  {
    _id: ObjectId("6405216ea8f69ebadb87ea45"),
    name: 'Felix',
    edad: 32
  }
]
appdb>
```

Aca vemos como podemos pasar un segundo argumento a `find()`, para especificar los campos que queremos consultar.

Si queremos obtener todos los usuarios que no tienen como hobby `senderismo`, podemos hacer:

```powershell
appdb> db.users.find({hobbies: {$ne: 'Senderismo'}})
[
  { _id: ObjectId("640516cca8f69ebadb87ea41"), name: 'Juan' },
  {
    _id: ObjectId("64051a61a8f69ebadb87ea42"),
    name: 'Anna',
    edad: 20,
    direccion: { calle: 'colon', numero: 2285 },
    hobbies: [ 'correr' ]
  },
  {
    _id: ObjectId("6405216ea8f69ebadb87ea43"),
    name: 'Pedro',
    edad: 26,
    hobbies: [ 'power lifting', 'Football' ],
    direccion: { calle: 'Ayolas', numero: 456, ciudad: 'Asuncion' }
  },
  {
    _id: ObjectId("6405216ea8f69ebadb87ea44"),
    name: 'Guillermo',
    edad: 41,
    hobbies: [ 'Natacion' ],
    direccion: { calle: 'Lilio', numero: 1245, ciudad: 'Asuncion' }
  }
]
appdb>
```

Vemos que no aparece Felix entre los users listados, porque el tiene como hobby ‘Senderismo’.

{: .concept}
Estas expresiones son Case Sensitive, esto significa que `Senderismo` no es lo mismo que `senderismo`


Existen muchas de estas expresiones para consultas complejas. Podemos ver una lista de las opciones que tenemos en este recurso de MongoDB: [https://www.mongodb.com/docs/manual/reference/operator/](https://www.mongodb.com/docs/manual/reference/operator/).

Para agregar un par de ejemplos mas, podemos consultar cuales son los usuarios que tengan mas de 30 años:

```powershell
appdb> db.users.find({edad: {$gt:30}})
[
  {
    _id: ObjectId("6405216ea8f69ebadb87ea44"),
    name: 'Guillermo',
    edad: 41,
    hobbies: [ 'Natacion' ],
    direccion: { calle: 'Lilio', numero: 1245, ciudad: 'Asuncion' }
  },
  {
    _id: ObjectId("6405216ea8f69ebadb87ea45"),
    name: 'Felix',
    edad: 32,
    hobbies: [ 'Ciclismo', 'Senderismo' ],
    direccion: { calle: 'Piribebuy', numero: 6521, ciudad: 'Asuncion' }
  }
]
appdb>
```

Podemos obtener los usuarios que viven en las calles Ayolas **O** Colon:

```powershell
appdb> db.users.find({"direccion.calle": {$in:["Ayolas", "colon"]}})
[
  {
    _id: ObjectId("64051a61a8f69ebadb87ea42"),
    name: 'Anna',
    edad: 20,
    direccion: { calle: 'colon', numero: 2285 },
    hobbies: [ 'correr' ]
  },
  {
    _id: ObjectId("6405216ea8f69ebadb87ea43"),
    name: 'Pedro',
    edad: 26,
    hobbies: [ 'power lifting', 'Football' ],
    direccion: { calle: 'Ayolas', numero: 456, ciudad: 'Asuncion' }
  }
]
appdb>
```

Un aspecto importante que vemos aca, es que “Ayolas” empieza con mayúsculas, y “colon” empieza con minúsculas. Esto es un problema de modelado de datos, y es el tipo de inconsistencias que tenemos que tratar de evitar, pero podemos resolver esto usando funciones de agregación, que veremos mas abajo.