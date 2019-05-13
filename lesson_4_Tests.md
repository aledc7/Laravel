# Lesson 4 - Tests

## Tests

Laravel cuenta con un componente que permite probar los proyectos de manera automatizada, a este componente podriamos llamarlo como __la capa de Test__.

Laravel incluye en cada directorio principal de cada proyecto que creemos una carpeta llamada __test__ la cual incluye otras dos carpetas llamadas __Feature__ y __Unit__ .   Ahora lo vamos a ver con detenimiento.   

 __Feature:__   
En la carpeta __Feature__ se van a escribir codigos que van a simular __Peticiones HTTP__ al Servidor.  

__Unit:__  
Por otro lado la carpeta __Unit__ es para escribir código de pruebas para probar partes individuales de nuestra Aplicación.  



Para comenzar, es posible ejecutar las pruebas en la consola con el siguiente comando

```php
vendor/bin/phpunit
```
esto ejecutará todas las controladoras dentro del a carpeta test que terminen en test.php.   
```php
alejandrodecastro@aledc:/Applications/MAMP/htdocs/laravel/training-aledc$ vendor/bin/phpunit
PHPUnit 6.5.14 by Sebastian Bergmann and contributors.

..                                                                  2 / 2 (100%)

Time: 157 ms, Memory: 12.00MB

OK (2 tests, 2 assertions)
```


Ahora bien, como este comando se utilizará varias veces, es una buena idea crear un Alias con este comando, de esta manera será  mas fácil invocar.

es posible crear cualquier alias simplemente escribiendo alias  nombre=comando

```php
alias test=vendor/bin/phpunit
```
En el ejemplo de arriba he creado un alias llamado __test__ que será el equivalente a escribir el comando completo: vendor/bin/phpunit    


luego al ejecutar el comando __test__ , Laravel va a recorrer la carpeta Test y ejecutara una prueba por cada archivo que encuentre con el nombre terminando en  Test.php

En el ejemplo que vimos arriba observamos que se han ejecutado dos test, estos corresponden a los dos archivos que Laravel genera de manera automatizada, uno es  __ExampleTest.php__ dentro de la carpeta __Feature__ y el mismo nombre de archivo __ExampleTest.php__ en la carpeta __Unit__.  



### Creando nuevos test

Para crear nuevos test que checkeen el correcto funcionamiento de las rutas se puede utilizar este comando:
```php
php artisan make:test TesteoModuloUsuariosTest
```

El comando de arriba generará un nuevo archivo llamado __TesteoModuloUsuariosTest__ dentro de la carpeta __text/Feature__  
Tener presente que si el nombre del archivo de testeno no termina en Test.php sensillamente no se ejecutará cuando corramos el test.

Ahora bien, las pruebas que se pueden hacer dentro de un archivo de test son casi infinitas... por lo que acá se dará un breve ejemplo de algunas de las cosas que es posible hacer:


## Ejemplo de un archivo de test que verifica si la ruta es accesible, y si devuelve el resultado que se espera que devuelva.

```php
<?php

namespace Tests\Feature;

use Tests\TestCase;
use Illuminate\Foundation\Testing\WithFaker;
use Illuminate\Foundation\Testing\RefreshDatabase;

class rutausuariosTest extends TestCase
{
    /**
     * A basic test example.
     *
     * @return void
     */
    public function testRutaUsers()
    {   // aca simulo una peticion del tipo get a la url indicada.
        $this->get('/usuarios/5')
            // aca le digo que la respuesta debe ser 200, o sea que si encuentre la url
            ->assertStatus(200)
            
            // aca le digo que la ruta debe mostrar esta cadena de texto.
            ->assertSee("Mostrando detalle del usuario: 5");
    }
}

```









