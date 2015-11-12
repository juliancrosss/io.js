# io.js

*Javascript del lado del servidor con io.js podemos crear aplicaciones web robustas y escalables.*

verificar con 

    iojs -v
    npm install io.js 
    npm use iojs-1.7.1
    
*Verificar la instalacion de node.js y manegador de paquetes*

    node -v
    npm -v
    
*iniciando la aplicacion con node , asociando el package.json las dependencias de mi aplicacion*

    npm init
    
    name: (animate-server) animado-server // nombre de la aplicacion
    version: (1.0.0)
    description: A realtime chat with video and gifs
    entry point: (index.js) server.js // Punto de entrada del la aplicacion
    test command: // Test 
    git repository: (https://github.com/julianduque/animate-server.git) https://github.com/juliancrosss/io.js.git
    keywords: Realtime Chat
    author: Julian Cruz <freakcruz@me.com>
    license: (ISC) MIT
    
*Ya con esto se crea package.json*

    package.json:

    {
        "name": "animado-server",
        "version": "1.0.0",
        "description": "A realtime chat with video and gifs",
        "main": "server.js",
        "scripts": {
            "test": "echo \"Error: no test specified\" && exit 1"
    },
        "repository": {
        "type": "git",
        "url": "git+https://github.com/juliancrosss/io.js.git"
    },
        "keywords": [
        "Realtime",
        "Chat"
    ],
        "author": "Julian Cruz <freakcruz@me.com>",
        "license": "MIT",
        "bugs": {
        "url": "https://github.com/juliancrosss/io.js/issues"
    },
        "homepage": "https://github.com/juliancrosss/io.js#readme"
    }

*Se crea un archivo para ignorar archivos binarios en un sistema de control de versiones como git o archivos como dependencias ya que pueden ser diferentes en otros sistemas operativos*

    vim .gitignore //no olvidar el punto al comienzo
    
*Escribimos dentro del archivo .gitignore los ficheros o archivos a ignorar*

    node_modules //fichero
    *.log //todos los archivos con la estenxion .log
    
*Archivos que tenemos hasta ahora*

    README.md    .gitignore    package.json
    
*La difecnia de git checkout con git diff 00..01* diferencia entre los commit

*git log ver la notas de los commits*

##Resumen

*Agregar package.json y archivo .gitignore*

    Para crear inicialmente el archivo package.json debemos ejecutar el
    comando `$ npm init`, este comando nos hará algunas preguntas iniciales
    sobre nuestro proyecto y es aquí donde definiremos las dependencias de
    nuestra aplicación.

    En el archivo .gitignore vamos a ignorar inicialmente la carpeta
    node_modules pues no es una buena práctica subir las dependencias al
    sistema de control de versiones.*
    
*Creamos el archivo de entrada server.js*

    vim server.js
    
*io.js tiene algunas caracteristicas de emacscript 6*

*Ecmascript 6*
    
    'use strict' // codigo que sigue es extricto siempre se escribe 

    const http = require('http') // const es una variable constante nunca cambia require de http
    const port = process.env.PORT || 8080 //definir otro puerto o el default 8080

    const server = http.createServer() // se crea el servidor 
    
    server.listen(8080)// donde esta escuchando el server
    server.listen(port)// puede ser este
    
*Ejecutando el servidor http*
    
    node server.js
    
*Verificando en otro terminal*

    curl http://localhost:8080
    
*Ejecutando el servidor desde otro puerto*

    PORT=8081 node server.js
    url http://localhost:8081
    
##Resumen

    Crear un servidor http muy básico

    En este commit hemos creado un servidor http completamente básico, tan
    básico que no hace nada, a partir de este servidor queremos explicar
    tres conceptos de programación asíncrona muy utilizados en io.js / Node.

    * Callbacks
    * Event Emitter
    * Streams
    
*Funciones anonimas "sin nombre" para realizar callback*

    'use strict'

    const http = require('http')
    const port = process.env.PORT || 8080

    const server = http.createServer(function (req, res){
        res.end('Hola io js')
    })
    server.listen(port, function (){
        console.log('Servidor escuchando en el puerto' + port)
    })

*Ejecutando server.js*

    node server.js
    url http://localhost:8080
    
*Ejecutando desde otro puerto*

    PORT=8081 node server.js
    
*Resumen*

    Agregar callback en el request handle y en el listen

    Aquí agregamos un par de callbacks a nuestro servidor, el primero es una
    función anónima que se ejecutará cada vez que llegue una petición http a
    nuestro servidor, el segundo se ejecutará cuando el servidor inicie por
    primer vez.

    Un callback es una función JavaScript que se ejecuta cuando una
    operación asíncrona ha terminado.
    
*Callbacks mas mantenibles*

    'use strict'

    const http = require('http')
    const port = process.env.PORT || 8080

    const server = http.createServer(onRequest)
    server.listen(port, onListening)


    function onRequest (req, res){
        res.end('Hola io.js')
    }
    function onListening(){
        console.log('Server running in port' + port)
    }
    
*Ejecutando en la terminal*

    node server.js
    curl http://localhost:8080
    
##Resumen

    Refactor: Extraer callback a funciones

    Una muy buena práctica para evitar el temido `callback hell` es utilizar
    named functions, esto es, declarar las funciones que servirán como tus
    callbacks y utilizarlas en vez de crear funciones anónimas para cada
    llamado asíncrono.

    De esta manera es incluso mas fácil hacer mantenimiento de tu código.
    
##Event Emitter

    'use strict'

    const http = require('http')
    const port = process.env.PORT || 8080

    const server = http.createServer()
    server.on('request', onRequest)
    server.on('listening', onListening)

    server.listen(port)

    function onRequest (req, res) {
        res.end('Hello io.js')
    }

    function onListening () {
        console.log('Server running in port ' + port)
    }
    
##Resumen

       Refactor: Cambiar de callback a event emitter

        Event Emitter es un concepto muy utilizado en io.js / Node el cual
        permite trabajar con eventos de una manera muy sencilla y permite que la
        programación asíncrona con io.js / Node sea muy poderosa.

        En este refactor es un poco mas claro (sin conocer el API), que la
        función `onRequest` se ejecutará cada que el evento `request` en el
        servidor sea llamado.

    
    
