# Clase 4

## Planning del día 

1. Repaso clase anterior
2. Introducción a NodeJS
3. Introducción a Babel
4. Introducción a Webpack
5. Creación de proyectos y scaffoldings
6. Añadiendo funcionalidades extra a nuestro proyecto
7. Los Single File Components y vue-loader
8. Ejercicio "TODO LIST usando vue-cli y SFC"

## Índice

* [1. Introducción a NodeJS](#1.-Introducción-a-NodeJS)
    * [1.1. ¿Qué es?](#1.1.-¿Qué-es?)
    * [1.2. ¿Para qué se puede utilizar?](#1.2.-¿Para-qué-se-puede-utilizar?)
    * [1.3. ¿Para qué nos es útil en Vue?](#1.3.-¿Para-qué-nos-es-útil-en-Vue?)
    * [1.4. ¿Qué es NPM?](#1.4.-¿Qué-es-NPM?)
    * [1.5. ¿Cómo se instala NodeJS y NPM?](#1.5.-¿Cómo-se-instala-NodeJS-y-NPM?)
    * [1.6. ¿Cómo se usa NPM?](#1.6.-¿Cómo-se-usa-NPM?)
* [2. Introducción a Babel](#2.-Introducción-a-Babel)
    * [2.1. ¿Qué es?](#2.1.-¿Qué-es?)
    * [2.2. ¿Qué es ES6](#2.2.-¿Qué-es-ES6)
    * [2.3. Nuevas funcionalidades](#2.3.-Nuevas-funcionalidades)
        * [2.3.1. Importación/Exportación de módulos](#2.3.1.-Importación/Exportación-de-módulos)
        * [2.3.2. Destructurización](#2.3.2.-Destructurización)
        * [2.3.3. `fetch`](#2.3.3.-`fetch`)
        * [2.3.4. `let` y `const`](#2.3.4.-`let`-y-`const`)
        * [2.3.5. `async/await`](#2.3.5.-`async/await`)
* [3. Introducción a Webpack](#3.-Introducción-a-Webpack)
    * [3.1. ¿Qué es?](#3.1.-¿Qué-es?)
    * [3.2. ¿Cómo se utiliza?](#3.2.-¿Cómo-se-utiliza?)
    * [3.3. Diferentes partes de Webpack](#3.3.-Diferentes-partes-de-Webpack)
        * [3.3.1. `entry`](#3.3.1.-`entry`)
        * [3.3.2. `output`](#3.3.2.-`output`)
        * [3.3.3. `loader`](#3.3.3.-`loader`)
        * [3.3.4. `plugins`](#3.3.4.-`plugins`)
    * [3.4. Ejercicio "Montando tu primer scaffolding"]()
* [4. Introducción a Vue Cli](#4.-Introducción-a-Vue-Cli)
    * [4.1. ¿Qué es?](#4.1.-¿Qué-es?)
    * [4.2. Diferentes partes](#4.2.-Diferentes-partes)
    * [4.3. Instalación](#4.3.-Instalación)
    * [4.4. Creando un proyecto](#4.4.-Creando-un-proyecto)
    * [4.5. Añadiendo plugins](#4.5.-Añadiendo-plugins)
    * [4.6. Usando variables de entorno](#4.6.-Usando-variables-de-entorno)
    * [4.7. Usando Vue GUI](#4.7.-Usando-Vue-GUI)
* [5. Single File components](#5.-Single-File-components)
    * [5.1. Partes de un SFC](#5.1.-Partes-de-un-SFC)
    * [5.2. Indicando el lenguaje](#5.2.-Indicando-el-lenguaje)
    * [5.3. Indicando el ámbito del CSS](#5.3.-Indicando-el-ámbito-del-CSS)
* [6. Ejercicio "TODO LIST usando vue-cli y SFC"](#6.-Ejercicio-"TODO-LIST-usando-vue-cli-y-SFC")


# 1. Introducción a NodeJS

Una vez que hemos visto todo lo relativo a la librería de VueJS dedicada al renderizado de vistas. Vamos a dedica tiempo a cómo escalar nuestro proyecto para una aplicación mayor... pero para llegar a ello, necesitamos tener conceptos básicos de algunas tecnologías. Empecemos por NodeJS

## 1.1. ¿Qué es?

NodeJS es una tecnología que nos permite interpretrar y ejecutar JavaScript en cualquier ordenador sin la necesidad de un navegador y con todas las herramientas (paquetes, librerías, APIS...) necesarias para poder obtener y almacenar los recursos de cualquier sistema en el que se encuentre.

NodeJS funciona gracias a la potencia del motor JavaScript v8 creado por Google, Open Source y que usa Google Chrome. 
    
## 1.2. ¿Para qué se puede utilizar?

A día de hoy. El poder ejecutar y tener acceso a los recursos de un ordenador por medio de JavaScript nos es muy útil por varias razones:

* Nos permite crear aplicaciones Web en el servidor: aplicaciones que pueden gestionar un número elevado de peticiones de manera asíncronas co grandes prestaciones de servicio.
* Nos permite realizar evaluaciones de ficheros JavaScript en nuestro propio terminal de comandos.
* Nos permite crear interfaces de lineas de comandos que extiendan las ya obtenidas por el sistema operativo.

## 1.3. ¿Para qué nos es útil en Vue?

Es este ultimo uso el que nos es útil en VueJS. El equipo de VueJS ha creado un proyecto llamado vue-cli, desarrollado con NodeJS que nos permite crear scaffoldings de manera automática por medio de configuraciones. 

## 1.4. ¿Qué es NPM?

NPM son las siglas de node package manager. Es una herramienta web y de línea de comandos que nos permite instalar en nuestro sistema operativo o proyecto utilidades creadas en JavaScript y NodeJS. Es también un gestor de dependencias y una de las primeras herramientas con la que nos pelearemos al usar NodeJS.

## 1.5. ¿Cómo se instala NodeJS y NPM?

Para instalar NodeJS y NPM, lo mejor es ir a la página oficial de NodeJS y descargar la versión LTS del binario. Una vez que instalemos con el instalador (esto dependerá del sistema operativo que tengamos, es instalable tanto en Windows como en distro Linux o Mac), podremos comprobar si todo se ha instalado bien.

Abrammos una linea de comandos o terminal y ejecutemos la siguiente linea para ver si tenemos NodeJS instalado:

```sh
$ node -v
```

Esto nos indicará la versión de NodeJS instalada. Si ejecutamos lo siguiente:

```sh
$ npm -v
```

Nos indicará la versión de NPM instalada.

Si no estamos en las versiones más nuevas de NPM, instalemosla por medio del siguiente comando:

```sh
$ npm install -g npm
```

## 1.6. ¿Cómo se usa NPM?

Como decíamos, NPM nos va a servir para instalar dependencias o paquetes de NodeJS.

Dentro de la carpeta en la que deseemos montar un proyecto de NodeJS, abriremos una terminal y empezaremos a crear nuestra aplicación.

Para iniciar una aplicación, deberemos ejecutar el siguiente comando:

```sh
$ npm init
```

Este comando nos realizará una serie de preguntas que deberemos contestar. Al finalizar, nos generará un fichero llamado `package.json`. Este fichero json es un fichero de configuración y metainformación de nuestro proyecto. Un ejemplo de fichero `package.json` podría ser el siguiente:

```js
{
  "name": "curso_vue_fictizia",
  "version": "1.0.0",
  "description": "ejemplo de paquete de nodejs",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "node",
    "vue",
    "curso",
    "fictizia"
  ],
  "author": "jdonsan",
  "license": "MIT"
}
```

Nos sirve para indicar el nombre, la descripción y versión de nuestro proyecto. Tambien nos va a servir para instalar dependencias.

Las dependencias de un proyecto de NodeJS se instalan en `node_modules`. Tenemos dos contextos donde poder instalarlas:

* A nivel global: nos sirve para guardar dependencias que usamos mucho y herramientas de línea de comandos.
* A nivel local: nos sirve para guardar las dependencias que usa un proyecto determinado y que no necesita compartir con el resto porque es de uso específico.

Para instalar estas dependencias, lo haríamos de esta manera. Si quisieramos instalar VueJS en nuestro proyecto. Lo haríamos de manera local. Sería así:

```sh 
$ npm install vue --save
```

Es decir instálame `vue` de manera local y como una dependencia que necesito en producción. 

Podemos instalar dependencias que solo se usarán en tiempo de desarrollo. `webpack` va a ser una de estas herramientas. Para instalarla. hacemos lo siguiente:

```sh
$ npm install webpack --save-dev
```

Lo único que cambia con la anterior es el flag del final.

Ahora bien, para instalar `vue-cli` que voy a hacer usos de ella en muchos proyectos diferentes, lo haría de manera global y esto se haría así:

```sh
$ npm install vue-cli -g
```

El flag `g` (de global), nos permite esto.

Para desinstalar dependencias, se usaría `uninstall` y nos funcionaría todos los flags que hemos indicado anteriormente.


# 2. Introducción a Babel
    
## 2.1. ¿Qué es?
    
## 2.2. ¿Qué es ES6

## 2.3. Nuevas funcionalidades
    
### 2.3.1. Importación/Exportación de módulos

### 2.3.2. Destructurización
    
### 2.3.3. `fetch`
        
### 2.3.4. `let` y `const`
    
### 2.3.5. `async/await`

# 3. Introducción a Webpack
    
## 3.1. ¿Qué es?
    
## 3.2. ¿Cómo se utiliza?

## 3.3. Diferentes partes de Webpack
        
### 3.3.1. `entry`

### 3.3.2. `output`

### 3.3.3. `loader`

### 3.3.4. `plugins`

## 3.4. Ejercicio "Montando tu primer scaffolding"

# 4. Introducción a Vue Cli

## 4.1. ¿Qué es?

## 4.2. Diferentes partes

## 4.3. Instalación

## 4.4. Creando un proyecto

## 4.5. Añadiendo plugins

## 4.6. Usando variables de entorno

## 4.7. Usando Vue GUI

# 5. Single File components

## 5.1. Partes de un SFC

## 5.2. Indicando el lenguaje

## 5.3. Indicando el ámbito del CSS

## 6. Ejercicio "TODO LIST usando vue-cli y SFC"

    