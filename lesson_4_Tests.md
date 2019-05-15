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

Es posible crear cualquier alias simplemente escribiendo __alias  nombre=comando__    

```php
alias test=vendor/bin/phpunit
```
En el ejemplo de arriba he creado un alias llamado __test__ que será el equivalente a escribir el comando completo: vendor/bin/phpunit    


Luego al ejecutar el comando __test__ , Laravel va a recorrer la carpeta Test y ejecutara una prueba por cada archivo que encuentre con el nombre terminando en Test.php

NOTA:  si el nombre del archivo no termina en Test.php sencillamente no se ejecutará cuando se corra el test.

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

### Es posible poner en un mismo archivo de test varias funciones que testeen varias rutas, de esta manera se evitará tener que crear un archivo por cada test.

para lograr esto, se pueden incluir varias funciones, anteponiendo adelante un bloque de codigo comentado un __@test__ como se muestra aqui:    

```php
     /**
     * Este comentario es opcional pero altamente recomendado, así podremos saber que está testeando la función.
     *
     @test
     */
```

##### Aqui un ejemplo completo de un controlador para test que realiza 6 test y verifica cada ruta definida, y que devuelva lo que se supone que tiene que devolver.  
En estos casosa de ejemplo se trata solo de strings.   
Observese que antes de cada función es necesario poner la linea de código comentada con el __@test__ 


```php
<?php

namespace Tests\Feature;

use Tests\TestCase;
use Illuminate\Foundation\Testing\WithFaker;
use Illuminate\Foundation\Testing\RefreshDatabase;

class TesteoRutaTest extends TestCase
{
    /**
     * Test de Ruta test
     *
     @test
     */
    function testRutaTest()
    {
        $this->get('/test')
            ->assertStatus(200)
            ->assertSee('test of new route 1');
    }
    
    /**
     * Test de Ruta usuarios
     *
     @test
     */
    function usuariosRutaTest()
    {
        $this->get('/usuarios')
            ->assertStatus(200)
            ->assertSee("Usuarios");
    }

    /**
     * Test de Ruta othertest
     *
     @test
     */
    function othertestRutaTest()
    {
        $this->get('/othertest')
            ->assertStatus(200)
            ->assertSee("Test of Another Route");
    }

    /**
     * Test de Ruta usuaruisID
     *
     @test
     */
    function usuariosId()
    {
        $this->get('/usuarios/5')
            ->assertStatus(200)
            ->assertSee("Mostrando detalle del usuario: 5");
    }

    /**
     * Test de Ruta usuarios2Id
     *
     @test
     */
    function usuarios2Id()
    {
        $this->get('/usuarios2/texto_x')
            ->assertStatus(200)
            ->assertSee("Mostrando detalle del usuario: texto_x");
    }

    /**
     * Test de Ruta whereId
     *
     @test
     */
    function whereId()
    {
        $this->get('/where/7')
            ->assertStatus(200)
            ->assertSee("Mostrando detalle del usuario: 7");
    }

    
    
}
```







