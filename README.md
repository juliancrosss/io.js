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

*Se crea un archivo para ignorar archivos binarios en un sistema de control de versiones*

    vim .gitignore
    
*Escribimos dentro del archivo .gitignore los ficheros o archivos a ignorar*

    node_modules //fichero
    *.log //archivos
    
    
