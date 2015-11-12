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
        
*Agregando archivos estaticos de forma sincronica*

    'use strict'

    const http = require('http')
    const fs = require('fs')
    const port = process.env.PORT || 8080

    const server = http.createServer()

    server.on('request', onRequest)
    server.on('listening', onListening)

    server.listen(port)

    function onRequest (req, res) {
        let file = fs.readFileSync('public/index.html')//let es variable en emacscript 6
        res.end(file)
    }

    function onListening () {
        console.log('Server running in port ' + port)
    }

*Creando el archivo estatico en la carpeta 'public/index.html'*

    <html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width-device-width">
    <title>Animate</title>
    </head>
    <body>
        <h1>hola io.js</h1>
    </body>
    </html>

*Ejecutando el servidor*

    node server.js
    
*Eliminar la carpeta de forma recursiva*

    rm -rf public/
    
##Resumen

    Creamos un servidor web estático muy básico

    En este commit creamos un servidor web estático muy básico, solo está
    sirviendo un archivo index.html de la carpeta public y estamos
    utilizando el modulo `fs` para leer el archivo. En este caso hemos
    utilizado la API síncrona de `fs` (lo cual no es recomendado) y hemos
    hecho otro par de faltas que es mejor que solucionemos en los próximos
    commits.
    
*Ejecutando el archivo estatico asincronamente*

    'use strict'

    const http = require('http')
    const fs = require('fs')
    const port = process.env.PORT || 8080

    const server = http.createServer()

    server.on('request', onRequest)
    server.on('listening', onListening)

    server.listen(port)

    function onRequest (req, res) {
        fs.readFile('public/index.html', function (err, file){
            if(err){
                return res.end(err.message)
            }
        res.end(file)
        })
    }

    function onListening () {
    console.log('Server running in port ' + port)
    
*Ruta realativa*

    'use strict'

    const http = require('http')
    const fs = require('fs')
    const path = require('path')
    const port = process.env.PORT || 8080

    const server = http.createServer()

    server.on('request', onRequest)
    server.on('listening', onListening)

    server.listen(port)

    function onRequest (req, res) {
        let fileName = path.join(__dirname, 'public', 'index.html')
        fs.readFile(fileName, function (err, file){
            if(err){
                return res.end(err.message)
            }
            res.end(file)
        })
    }
    
##Resumen

    Refactor: Utilizar path y fs asíncrono

    En este commit utilizamos el modulo `path` para manejo de rutas de
    archivos, es muy mala práctica manejar estas rutas sin este modulo pues
    hay sistemas operativos (como Windows) que manejan los paths diferentes.

    También hemos cambiado el llamado del archivo de síncrono a asíncrono y
    enviamos la cabecera `Content-Type` en la petición http, poco a poco
    nuestro servidor va cogiendo mejor forma.
    
##Streams

*Como se envia la informacion*

    'use strict'

    const http = require('http')
    const fs = require('fs')
    const path = require('path')
    const port = process.env.PORT || 8080

    const server = http.createServer()

    server.on('request', onRequest)
    server.on('listening', onListening)

    server.listen(port)

    function onRequest (req, res) {
        let index = path.join(__dirname, 'public', 'index.html')

        res.setHeader('Content-Type', 'text/html')
        let rs = fs.createReadStream(index)
        rs.pipe(res)

        rs.on('error', function (err){
        rs.end(err.message)
    })
    
    
##Resumen

    Refactor: Utilizar streams en `fs`

    En este commit hacemos un refactor para utilizar Streams, uno de los
    conceptos mas poderosos de io.js / Node.

    En el anterior paso el archivo de carga todo en el buffer y cuando este
    termina de cargarse lo pasa al response cuando esta listo. Con streams a
    medida que se va leyendo el archivo este lo va pasando al responde y el
    performance es mucho mejor (Claro esta, que es visto cuando se trabaja
    con archivos mas grandes)

    A medida de este curso vamos a utilizar los 3 conceptos: callbacks,
    event emitter y streams para potenciar nuestra programación asíncrona
    con io.js
    
    
*Manego de string en emacscript 6*

    'use strict'

    const http = require('http')
    const fs = require('fs')
    const path = require('path')
    const port = process.env.PORT || 8080

    const server = http.createServer()

    server.on('request', onRequest)
    server.on('listening', onListening)

    server.listen(port)

    function onRequest (req, res) {
        let index = path.join(__dirname, 'public', 'index.html')
        let rs = fs.createReadStream(index)

        res.setHeader('Content-Type', 'text/html')
        rs.pipe(res)

        rs.on('error', function (err) {
            res.setHeader('Content-Type', 'text/plain')
            res.end(err.message)
        })
    }

    function onListening () {
        console.log(`Server running in port ${port}`)// se manega con ` y las variables con ${port}
    }
    
##Resumen

    Refactor: Usar template strings (ES6)

    Template strings es una caracteristica de ES6, con este ya podemos
    concatenar información a un string de una manera mucho mas limpia, este
    es un pequeño refactor educativo ;)
    
    rm -rf public/app.js //borrar datos
    
    Broken: Intentar servir un archivo js en el cliente

    Nuestra aplicación también necesitará serviro archivos en el cliente, no
    solo html sino css y js, en este commit lo intentamos pero vemos que
    nuestro servidor no es lo suficientemente inteligente para entregarnos
    el archivo app.js que necesitamos.
    
*Ejecutando otros archivos estaticos*

        'use strict'

        const http = require('http')
        const fs = require('fs')
        const path = require('path')
        const port = process.env.PORT || 8080

        const server = http.createServer()

        server.on('request', onRequest)
        server.on('listening', onListening)

        server.listen(port)

        function onRequest (req, res) {
            let uri = req.url

            if (uri.startsWith('/index') || uri === '/') return serveIndex(res)

                if (uri.startsWith('/app.js')) return serveApp(res)

                    res.statusCode = 404
                    res.setHeader('Content-Type', 'text/plain')
                    res.end(`404 Not found: ${uri}`)
                }

            function serveIndex (res) {
            let index = path.join(__dirname, 'public', 'index.html')
            let rs = fs.createReadStream(index)

            res.setHeader('Content-Type', 'text/html')
            rs.pipe(res)

            rs.on('error', function (err) {
            res.setHeader('Content-Type', 'text/plain')
            res.end(err.message)
        })
    }

    function serveApp (res) {
        let app = path.join(__dirname, 'public', 'app.js')
        let rs = fs.createReadStream(app)

        res.setHeader('Content-Type', 'text/javascript')
        rs.pipe(res)

        rs.on('error', function (err) {
        res.setHeader('Content-Type', 'text/plain')
        res.end(err.message)
    })
    }

    function onListening () {
        console.log(`Server running in port ${port}`)
    }


##Resumen
    
    Fix: Servir index y app.js sin problemas

    En este commit creamos dos metodos `serveIndex` y `serveApp`, estos
    serán utilizados para servir nuestro archivo index y javascript, pero...
    si seguimos este patrón nuestro código empezará a ser muy dificil de
    mantener de ahora en adelante.

    En los siguientes pasos vamos a ver como podemos agregar es lio sin
    necesidad de utilizar un framework completo, para esto usaremos
    librerias que solucionan problemas especificos como el de servir
    archivos estaticos y manejar rutas
    
*Instalando un modulo para servir archivos estaticos en el servidor node o io*

    npm install st --save // --save , sava la dependencia en nuestro package.json
    
*Mejorando el codigo con un dentro de la carpeta router para routear las urls*

    router/index.js
    
    const path = require('path')
    const st = require('st')

    const mount = st({
        path: path.join(__dirname, '..', 'public'),
        index: 'index.html'
    })

    function onRequest (req, res) {
        mount(req, res, function (err) {
        if (err) return fail(err, res)

            res.statusCode = 404
            res.end(`404 Not Found: ${req.url}`)
        })
    }

    function fail (err, res) {
        res.statusCode = 500
        res.setHeader('Content-Type', 'text/plain')
        res.end(err.message)
    }

    module.exports = onRequest
    
    
*Dentro de server.js*

    'use strict'

    const http = require('http')
    const router = require('./router')

    const server = http.createServer()
    const port = process.env.PORT || 8080

    server.on('request', router)
    server.on('listening', onListening)

    server.listen(port)

    function onListening () {
        console.log(`Server running in port ${port}`)
    }
    
    
##Resumen 
    
    
    Refactor: Utilizar `st` para manejo de archivos estáticos

    En este commit creamos un modulo llamado `router`, en este modulo vamos
    a manejar todo lo relacionado con las rutas y archivos de nuestra
    aplicación, inicialmente utilzamos `st` un modulo que me permite montar
    una carpeta como estática en mi proyecto web ahora cualquier tipo de
    archivo que se encuentre en la carpeta `public` será servido por este
    módulo, esto nos ha reducido complejidad sin perder control sobre como
    nuestra aplicación está funcionando.
    
