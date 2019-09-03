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
