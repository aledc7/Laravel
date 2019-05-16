# Lesson 3 - Routes

## Routes in Laravel

Begin to define our first routes, as we saw in the last chapter, we must edit the file:
```php
/routes/web.php      
```
Note that if it were an API we should modify the file corresponding to the APIs, this is:
```php
/route/api.php  
```
We could say that the routes are our  personalized "url addresses" that we define. The routes are defined as follows:    

```php

Route::get('/', function () {
    return view('welcome');
});


Route::get('/test',function (){
    return 'test of new route 1';
});

Route::get('/othertest',function (){
    return 'test of another route';
});

```

Above we have defined three different routes for our project, one will be the home page('/') and will return the welcome view     
Other one called test ('/test'), and will return a string 'test of another rute'
and third call othertest('/othertest') .  
In the cited example, these routes return a string of text, but the idea is to point to a component, which will be a section of our project.



```php
// route example that returns a parameter passed
Route::get('/usuarios/{id}', function($id){
    return "Mostrando detalle del usuario: {$id}";
});


/* 
 Other example with a condition:
 in this example I create a route in which two parameters, name and nickname must be passed
 The sign ? which is passed to the nickname parameter means that it is optional,   
 if that sign is not passed, and the parameter is not passed, it will give an error.   
*/
Route::get('/Hola/{name}/{nickname?}', function($name, $nickname = null){
    if ($nickname) {
        return "Welcome {$name}, your nickname is: {$nickname}";
    } else {
        return "Welcome {$name}, you don't set a nick name...";
    }
});
```



## Example of web.php file with coments in my native Spanish
```php
/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/


/*
|--------------------------------------------------------------------------
| Rutas en Laravel
|--------------------------------------------------------------------------
| Laravel tiene un potente Framework , que maneja las rutas por vos.
| Simplemente se declara la ruta, y luego hay una función que hace lo que necesites
| desde devolver un string, un objeto, devolver una vista, o lo que se necesite.
| Es posible usar "parámetros dinámicos" entre llaves para obtener parámetros en la url, que luego usará la funcion de cada ruta.
| Estos parametros dinámicos se pasan usando las llaves { } y dentro la palabra o id que quieras poner.
| Es posible pasar en la url mas de un parámetro, como veremos en el ejemplo siguiente.
| Tambien es posible crear cualquier lógica con cualquier estructura que maneje php como ser if, else, etc.
|  
| Lo mas comun es usar el framework de las rutas solo para devolver una View, usando un return.
| Esto se cubrirá en su totalidad mas adelante.
*/



// en Laravel el orden de las rutas es Fundamental, Laravel tomará la primer
// ruta que coincida y el resto simplemente las ignora.


// Esta ruta es la ruta principal del HOME, y devuelve una vista llamada welcome
Route::get('/', function () {
    return view('welcome');
});

// aca tenemos la ruta /test que devuelve una cadena de texto 'test of new route 1'
Route::get('/test',function (){
    return 'test of new route 1';
});

// otro ejemplo de ruta llamada /othertest que también devuelve una cadena te texto.
Route::get('/othertest',function (){
    return 'test of another route';
});


// este ejemplo pasa un parametro, que luego será devuelto por la funcion anonima.
Route::get('/usuarios/{id}', function($id){
    return "Mostrando detalle del usuario: {$id}";
});


// ejemplo igual al anterio, pasa un parametro, que luego será devuelto por la funcion anonima.
Route::get('/usuarios2/{mi_parametro}', function($mi_parametro){
    return "Mostrando detalle del usuario: {$mi_parametro}";
});

// Ejemplo en donde uso un "where" para limitar a que en la url solo se puedan poner numeros
// del 1 al 9 y mas de un numero (+)
Route::get('/where/{mi_parametro}', function($mi_parametro){
    return "Mostrando detalle del usuario: {$mi_parametro}";
})->where('mi_parametro','[0-9]+');






/* En este ejemplo creo una ruta en la que se deben pasar dos parametros.  
| El signo ? que se le pasa al parametro 'apodo' significa que es opcional.   
| Si no se le pasa ese signo, y el parametro no es pasado, dará error.
| Observese que dentro de la función anónima, el parámetro 'apodo' es definido 
| como null, para que no de error.
| por último, como esto es php de fondo, se puede usar cualquier lógica que queramos
| en este caso uso un "if else"  
*/ 

Route::get('/Hola/{nombre}/{apodo?}', function($nombre, $apodo = null){
    if ($apodo) {
        return "Bienvenido {$nombre}, tu apodo es: {$apodo}";
    } else {
        return "Bienvenido {$nombre}, No pasaste ningun parametro como apodo";
    }
});



// lo mismo que arriba pero con etiquetas html para hacer grande el texto y darle estilo.
Route::get('/Holaestilo/{nombre}/{apodo?}', function($nombre, $apodo = null){
    if ($apodo) {
        return "<h1>Bienvenido {$nombre}, tu apodo es: {$apodo} </h1> ";
    } else {
        return "<h1>Bienvenido {$nombre}, No pasaste ningun parametro como apodo</h1>";
    }
});
```

## Archivo composer.json

Este es el archivo principal para instlar laravel, y también es el que se utiliza para el manejo de dependencias del proyecto y sirve para Autocargar las Clases.

### Ejemplo del archivo composer.json
```php
{
    "name": "laravel/laravel",
    "description": "The Laravel Framework.",
    "keywords": ["framework", "laravel"],
    "license": "MIT",
    "type": "project",
    "require": {
        "php": ">=7.0.0",
        "fideloper/proxy": "~3.3",
        "laravel/framework": "5.5.*",
        "laravel/tinker": "~1.0"
    },
    "require-dev": {
        "filp/whoops": "~2.0",
        "fzaninotto/faker": "~1.4",
        "mockery/mockery": "~1.0",
        "phpunit/phpunit": "~6.0",
        "symfony/thanks": "^1.0"
    },
    // ESTO ES TODO LO QUE SE MAPEA
    "autoload": {
        "classmap": [
            "database/seeds",
            "database/factories"
        ],
        // ACA esta mapeando todas las clases que comiencen con el namespace App, a la carpeta app
        "psr-4": {
            "App\\": "app/"
        }
    },
    "autoload-dev": {
        // ACA esta mapeando todas las clases que comiencen con el namespace Test, a la carpeta test
        "psr-4": {
            "Tests\\": "tests/"
        }
    },
    "extra": {
        "laravel": {
            "dont-discover": [
            ]
        }
    },
    "scripts": {
        "post-root-package-install": [
            "@php -r \"file_exists('.env') || copy('.env.example', '.env');\""
        ],
        "post-create-project-cmd": [
            "@php artisan key:generate"
        ],
        "post-autoload-dump": [
            "Illuminate\\Foundation\\ComposerScripts::postAutoloadDump",
            "@php artisan package:discover"
        ]
    },
    "config": {
        "preferred-install": "dist",
        "sort-packages": true,
        "optimize-autoloader": true
    }
    ```
    
[Go to Lesson 4 ->](https://github.com/aledc7/Laravel/blob/master/lesson_4_Tests.md)


