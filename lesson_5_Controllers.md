![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)
# Lesson 5 - Controllers

# Controladores en Laravel

Laravel cuenta con una capa denominada __CONTROLLER__ que permite __agrupar__ peticiones __http__ dentro de __clases__ y a su vez dividirlas en varios __métodos__.



Laravel agrupa toda la lógica en una capa __Controlers__ que contendrá la Lógica de nuestra Aplicación, entre otras cosas, peticiones HTTP relacionadas dentro de una clase, y dividirlas en varios métodos.


### Ubicacion de los controladores:
```php
/app/Http/Controllers/
```

### Como Generar un Controlador:
```php
php artisan make:controller ControladorPrueba
```

Si todo fue bien, se tiene que haber creado el archivo:
```php
/app/Http/Controllers/ControladorPrueba.php
```
Un controlador no es más que un archivo .php con una clase que extiende de la clase App\Http\Controllers\Controller:
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class ControladorPrueba extends Controller
{
    //
}
```

La controladora de arriba recien creada es una __Clase__ de php.


La clase tiene un __namespace__  que sigue el patron __PSR4__   


> 
> PSR-4 fue creada por el grupo de interoperabilidad de PHP. Este grupo ha trabajado en la creación de especificaciones que nos permitan a nosotros, desarrolladores de PHP, estandarizar muchos procesos, como, en este caso, la manera como nombramos a las clases.
> 

### Que es un namespace

Arriba vemos en la controladora recien creada que aparece __namespace__ y luego una ruta.   
Esta ruta será la carpeta en donde exista la controladora.  De esta manera es posible crear dos clases con el mismo nombre pero en ubicaciones distintas.


## Ejemplo de Controladora
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class ShowController extends Controller
{
    public function show($id)
    {
        return "Mostrando detalle del usuario: {$id}";
    }
}
```


## Enlazar una ruta a un controlador
Para enlazar una ruta a un controlador pasamos como argumento el nombre del controlador y del método que queremos enlazar, separados por un @. En este caso queremos enlazar la ruta /usuarios al método show del controlador ShowController:
```php
Route::get('/usuarios/{id}', 'ShowController@show');
```




## Logs en Laravel

Laravel tiene un LOG en donde se registrará cada error que ocurra, este es centralizado en el archivo:
```php
/storage/logs/laravel.log
```
El archivo de Log puede tener gran cantidad de información, por lo que es una buena práctica borrar su contenido antes de intentar debugear un error.
Luego al abrir el archivo nos encontramos con mucha info, aunque la info mas relevante estará en la primer linea.   
Aqui pongo este ejemplo de la primer linea de error, que me está diciendo que tengo una variable no definida, tambien me dice el numero de linea, y el archivo con su ubicación.  
```php
[2019-05-16 14:54:59] testing.ERROR: Undefined variable: paramet11 {"exception":"[object] (ErrorException(code: 0): Undefined variable: paramet11 at /Applications/MAMP/htdocs/laravel/training-aledc/app/Http/Controllers/UserController.php:36)
[stacktrace]
```

También tenemos la posibilidad de visualizar esta informacion al momento de correr el test. esto se logra llamando a un método dentro del archivo de TEST que hayamos definido previamente.  

Este es el método que debemos incluír en cada función de nuestro archivo de test:   
```php
$this->withoutExceptionHandling();
```

Aquí el ejemplo copmpleto de como quedaría la función de test con la invocación al metodo:
```php
/**
* Test de Ruta whereId
*
@test
*/
function whereId()
{
    // Este método hace que al ejecutar el TEST informe el error con un detalle mayor (igual que en el log)
    $this->withoutExceptionHandling();
    
    $this->get('/where/7')
        ->assertStatus(200)
        ->assertSee("Mostrando detalle del usuario: 7");
}
```



Entonces al ejecutar el Test nos dara una informacion mas detallada, indicando el error, y el nombre de la prueba que disparo el error

```php
FAILURES!
Tests: 6, Assertions: 11, Failures: 1.
alejandrodecastro@aledc:/Applications/MAMP/htdocs/laravel/training-aledc$ vendor/bin/phpunit
PHPUnit 6.5.14 by Sebastian Bergmann and contributors.

.....E                                                              6 / 6 (100%)

Time: 181 ms, Memory: 12.00MB

There was 1 error:

1) Tests\Feature\TesteoRutaTest::whereId
ErrorException: Undefined variable: paramet11
```
## Controlador de un solo método
Si quieres tener un controlador que solo tenga una acción, puedes hacerlo llamando al método **__invoke**
Es posible crear un controlador que solo tenga una única función, esto se logra cambiando el nombre del método por la palabra **__invoke** entonces ponemos esa palabra en lugar del nombre del método de la función publica...  acá un ejemplo:
```php
    public function __invoke($paramet1)
    {
        return "Mostrando detalle del usuario: {$paramet1}";
    }
```
Luego en el archivo de rutas __web.php__ ya NO se debe poner el nombre __controlador@nombre_metodo__  sino solamente el nombre de la clase, como muestro aqui:
```php
Route::get('/where/{mi_parametro}', 'WhereController')
```
