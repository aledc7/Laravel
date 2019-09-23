![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)



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
