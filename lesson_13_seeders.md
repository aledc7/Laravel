![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)

# Capitulo 13 - Seeders

## Insertando Datos con Seeders


## Generar un seeder
Para generar un seeder se utiliza el comando de Artisan __make:seeder__ seguido del nombre del seeder:

```php
php artisan make:seeder ProfessionSeeder
````

Al ejecutar este comando se generará un archivo __ProfessionSeeder.php__ dentro del directorio database/seeds.

## Código del seeder
Dentro del método run() del archivo ProfessionSeeder.php se deberá escribir el código del seeder que se acaba de crear:

```php
class ProfessionSeeder extends Seeder
{
    public function run()
    {
        DB::table('professions')->insert([
            'title' => 'Desarrollador back-end',
        ]);
    }
}
````

Para insertar datos, se utilizará el constructor de consultas SQL de Laravel. Este constructor incluye una interfaz fluida para construir y ejecutar consultas a la base de datos. Para ello se llama al método table del Facade DB pasando como argumento el nombre de la tabla con la que se quiere interactuar.    
El método __insert__ acepta un array asociativo que reprensetará las columnas y valores que se quieran guardar en la tabla.

Para utilizar el facade DB:: se deberá importar __\Illuminate\Support\Facades\DB__ al principio del archivo:

```php
<?php

// (namespace opcional aqui)

use Illuminate\Support\Facades\DB;
````


## Registrar seeder
Los seeders son registrados en la clase DatabaseSeeder dentro de database/seeds/DatabaseSeeder.php.  
Dentro del método run llamamos al método call pasando como argumento el nombre de la clase de nuestro seeder:
```php
class DatabaseSeeder extends Seeder
{
    public function run()
    {
        $this->call(ProfessionSeeder::class);
    }
}
````

En este caso ProfessionSeeder::class devolverá el nombre de la clase.  
En lugar de utilizar ::class también es posible pasar el nombre de la clase como una cadena de texto 'ProfessionalSeeder'.

## Eliminar registros

Es posible que antes de ejecutar un seeder sea necesario eliminar el contenido existente.  
Para realizar esto se debe utilizar el método __truncate__, que se encarga de vaciar la tabla:

```php
class ProfessionSeeder extends Seeder
{
    public function run()
    {
        DB::table('professions')->truncate();

        // ..
    }
}
````

## Ejecutar un seeder
Para ejecutar los seeders se utiliza el comando __db:seed__ desde la terminal:

```php
php artisan db:seed
````
En caso de que se tenga múltiples seeders, es necesario pasar la opción __--class__ que permite ejecutar solamente el seeder pasado como argumento:
```php
php artisan db:seed --class=ProfessionSeeder
````

También es posible ejecutar el comando __migrate:fresh__ o migrate:refresh junto con los seeders pasando la opción __--seed__:
```php
php artisan migrate:fresh --seed
````

## Desactivar Revisión de Claves Foráneas

Si una tabla tiene una referencia de clave foránea, es necesasrio desactivar la revisión de claves foráneas utilizando un sentencia antes de vaciar dicha tabla (por ejemplo usando el método truncate).

Esto se lograr con la sentencia __SQL SET FOREIGN_KEY_CHECKS=[0|1]__.   
En Laravel se puede ejecutar dicha sentencia usando el método __DB::statement__ de esta manera:

```php
class ProfessionSeeder extends Seeder
{
    public function run()
    {
        DB::statement('SET FOREIGN_KEY_CHECKS = 0;'); // Desactivamos la revisión de claves foráneas
        DB::table('professions')->truncate();
        DB::statement('SET FOREIGN_KEY_CHECKS = 1;'); // Reactivamos la revisión de claves foráneas

        // ..
    }
}
````

Utilizando la misma sentencia pero con el valor __1__ se reactiva la revisión de claves foráneas luego de ejecutar el seeder.    
En caso de que se quiera vaciar varias tablas a la vez, se deberá utilizar el siguiente código dentro de la clase DatabaseSeeder:

```php
class DatabaseSeeder extends Seeder
{
    public function run()
    {
        $this->truncateTables([
            'nombre_de_la_tabla_aqui',
            'nombre_de_otra_tabla',
        ]);
  
        // Ejecutar los seeders:
        $this->call(ProfessionSeeder::class);
    }

    public function truncateTables(array $tables)
    {
        DB::statement('SET FOREIGN_KEY_CHECKS = 0;');

        foreach ($tables as $table) {
            DB::table($table)->truncate();
        }

        DB::statement('SET FOREIGN_KEY_CHECKS = 1;');
    }
}
````

Ahora ya será posible llamar al método __truncateTables__ pasando un arreglo con los nombres de las tablas que quieras vaciar.

## Contraseñas dentro de un seeder
En caso de que se quiera insertar usuarios de esta manera, hay que tener presente encriptar las contraseñas utilizando el helper __bcrypt__:
```php
DB::table('users')->insert([
    // ..
    'password' => bcrypt('laravel')
]);
```

[Ir a la siguiente leccion -->]()
