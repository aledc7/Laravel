# Lesson 5 - Controllers

## Controladores en Laravel


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
Con la siguiente estructura que enseguida explicaré:
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











