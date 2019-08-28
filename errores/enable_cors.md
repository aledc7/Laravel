![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)


# Habilitando Cors

En caso de obtener el error __has been blocked by CORS policy__  esto puede solucionarse habilitando CORS del lado del servidor.


- 1 - Instalar con composer la dependencia __laravel-cors__  

```php
composer require barryvdh/laravel-cors 
````

- 2 -  Editar el archivo __app/http/kernel.php__  y en el array __$middleware__ agregar la linea como se muestra:
```php
protected $middleware = [
    # Agregar esta linea
    \Barryvdh\Cors\HandleCors::class,


    \Illuminate\Foundation\Http\Middleware\CheckForMaintenanceMode::class,
    \Illuminate\Foundation\Http\Middleware\ValidatePostSize::class,
    \App\Http\Middleware\TrimStrings::class,
    \Illuminate\Foundation\Http\Middleware\ConvertEmptyStringsToNull::class,
    \App\Http\Middleware\TrustProxies::class,
];
````

- 3 - Publicar el cambio corriendo el siguente comando en la terminal del proyecto Laravel:
```php
php artisan vendor:publish --provider="Barryvdh\Cors\ServiceProvider"
````

- 4 - Por último, editar el archivo __config/cors.php__ y agregar la url que queremos que permita CORS.   
 En caso de querer permitir todo, colocar un __*__ para permitir todo.   
 Esto en la linea __'allowedOrigins' => ['*']__   
```php
return [

    /*
    |--------------------------------------------------------------------------
    | Laravel CORS
    |--------------------------------------------------------------------------
    |
    | allowedOrigins, allowedHeaders and allowedMethods can be set to array('*')
    | to accept any value.
    |
    */
   
    'supportsCredentials' => false,
    'allowedOrigins' => ['*'],   // en caso de que no quiera permitir todo, reemplazar el * por la url a permitir.
    'allowedOriginsPatterns' => [],
    'allowedHeaders' => ['*'],
    'allowedMethods' => ['*'],
    'exposedHeaders' => [],
    'maxAge' => 0,

];
````


Eso es todo.... ahora no recibiran mas el error de CORS
