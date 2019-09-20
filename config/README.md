![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)


# Configuraciónes de Laravel

###### Esta seccion contiene las distintas posibles configuraciónes de Laravel



## Setear el Server en un puerto distinto al 8000


En caso de que se quiera cambiar el puerto predeterminado, que viene seteado en el 8000, esto puede hacerse de dos maneras

1 - Provisoria, la cual lo cambiará solo por esa ejecucion
```php
php artisan serve --port=7342
````

2 - Fija, la cual lo dejará siempre en el puerto que configuremos
  - Dentro del proyecto de Laravel que querramos modificar, editar el archivo __ServeCommand.php__
```php
sudo nano vendor/laravel/framework/src/Illuminate/Foundation/Console/ServeCommand.php
````

  - Luego sobre el final del archivo, modificar el puerto 8000 por el que se desee:
  
```php
 protected function getOptions()
    {
        return [
            ['host', null, InputOption::VALUE_OPTIONAL, 'The host address to serve the application on.', '127.0.0.1'],

            ['port', null, InputOption::VALUE_OPTIONAL, 'The port to serve the application on.', 7342],
        ];
    }
````


____________________________________________________________________________________________________________________
# Cambiar Zona Horaria de Laravel

Editar el archivo __/config/app.php__   y setear la linea correspondiente a la zona horaria deseada:

```php
    /*
    |--------------------------------------------------------------------------
    | Application Timezone
    |--------------------------------------------------------------------------
    |
    | Here you may specify the default timezone for your application, which
    | will be used by the PHP date and date-time functions. We have gone
    | ahead and set this to a sensible default for you out of the box.
    |
    */


    'timezone' => 'America/Argentina/Buenos_Aires',

````
____________________________________________________________________________________________________________________
# Personalizar Color de fuente de los errores del Tinker

Tinker tiene configurado el color de la fuente GRIS con fondo ROJO...  esto es un error de alguien, ya que no es cómodo ni legible.


para corregir esto es necesario editar dentro de cada proyecto el archivo __ShellOutput.ph__  aca se muestra como:


```php
./vendor/psy/psysh/src/Output/ShellOutput.php

# En la linea 178 cambiar 'red' por 'white'

// $formatter->setStyle('error', new OutputFormatterStyle('black', 'red', ['bold']));
$formatter->setStyle('error', new OutputFormatterStyle('black', 'white', ['bold']));
````
_________________________________________________________________________________________________________________

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
_________________________________________________________________________________________________________________


## Cambiar el idioma de los errores

1 - Descargar la carpeta [es](https://github.com/aledc7/Laravel/tree/master/resources/es) y colocarla dentro del directorio:
```php
/resources/lang/
````
2 - Editar el archivo __config/app.php__  y cambiar el valor de __locale => 'en'__ por __es__   
```php
'locale' => 'es',
````
Eso es todo
_________________________________________________________________________________________________________________
## Instalar Passport

1 - Para comenzar, instalar Passport a través del administrador de paquetes Composer:    
```php
composer require laravel/passport
````
2 - Ejecutar migraciones de la base de datos para que Laravel cree las tablas necesarias de Passport
```php
php artisan migrate:fresh --seed
````

3 - Luego crear las claves de cifrado necesarias para generar tokens.
```php
php artisan passport:install


# Si todo fue bien el resultado debe ser el siguiente:

Encryption keys generated successfully.
Personal access client created successfully.
Client ID: 1
Client secret: ABCDEFGugcV6CcjAPzp2ScH6VVzoJ8YLkHIJKLMN
Password grant client created successfully.
Client ID: 2
Client secret: QWERTYYOTnROMWZrqBjyzmy7XltwgRREREGVLKHL
````


4 - Agregar estas lineas al modelo User:
```php

namespace App;

use Laravel\Passport\HasApiTokens;
use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    use HasApiTokens, Notifiable;
}

````

5 - Agregar las dos lineas faltantes en el archivo __ /app/Providers/AuthServiceProvider.php__
```php
<?php

namespace App\Providers;

# Agregar esta linea para implementar Passport
use Laravel\Passport\Passport;

use Illuminate\Support\Facades\Gate;
use Illuminate\Foundation\Support\Providers\AuthServiceProvider as ServiceProvider;

class AuthServiceProvider extends ServiceProvider
{
    /**
     * The policy mappings for the application.
     *
     * @var array
     */
    protected $policies = [
        'App\Model' => 'App\Policies\ModelPolicy',
    ];

    /**
     * Register any authentication / authorization services.
     *
     * @return void
     */
    public function boot()
    {
        $this->registerPolicies();

        # Agregar esta otra linea para implementar Passport
        Passport::routes();
    }
}

````


6 - Luego, se debe configurar el archivo __/config/auth.php__ de la siguiente manera:  
```php

'guards' => [
    'web' => [
        'driver' => 'session',
        'provider' => 'users',
    ],

    'api' => [
        'driver' => 'passport',
        'provider' => 'users',
    ],
],

````

7 - Finalmente se debe ejecutar este comando para usar los componentes de VUE para passport:
```php
php artisan vendor:publish --tag=passport-components
````





