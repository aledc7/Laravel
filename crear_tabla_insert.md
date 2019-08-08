![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)

# Creando Tablas con Migraciones e Insertando Datos 

Se explicará como crear una tabla a traves de las __migrations__, luego crear el __model__ correspondiente a esa tabla, y por ultimo cargar datos en la tabla, a traves de los __seeders__.


1 -   Lo primero será crear la __migration__ con el nombre de la tabla que queremos crear, es fundamental que el nombre contenga las palabras __create__ y __table__

```php
php artisan make:migration create_nombredelatabla_table
````

1.2 - Editar la migración recien creada en /database/migrations, y completar con la estructura de la tabla que queremos crear:

```php
Schema::create('users', function (Blueprint $table) {
            $table->increments('id');
            $table->string('name');
            $table->string('lastname')->nullable();
            $table->string('direccion')->nullable();
            $table->integer('cuit')->nullable();
            $table->integer('profesion_id')->nullable();
            $table->string('email')->unique()->nullable();
            $table->string('password');
            $table->string('notas')->nullable();
            $table->integer('dni')->nullable();
            $table->rememberToken();
            $table->timestamp('au_fecha_hora');
        });
````
2 - Crear el Modelo que corresponderá a la tabla creada en el paso 1:
```php
php artisan make:model NombreModelo
````
2.1 - Una vez creado el modelo, editarlo y se debe especificar el nombre exacto de la tabla, e indicar que se usará ese modelo:

```php
namespace App;

use App\NombreModelo;
use Illuminate\Database\Eloquent\Model;

class Menu extends Model
{
    protected $table = 'nombre_tabla';
}

````

3 - Una vez tenemos creada la migración y el modelo, creamos el seeder:
```php
php artisan make:seeder NombretablaSeeder
```

3.1 - Luego editamos el seeder que se habrá generado en la carpeta /database/seeds/  y tenemos que indicarle que use el modelo que ceamos en el paso 2. 
Se muestra como ejemplo un seeder para crear la tabla de profesiones

```php

# indico que voy a usar el modelo Profession
use App\Profession;
use Illuminate\Database\Seeder;

class ProfessionSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {

        Profession::create([
            'titulo' => 'Desarrollador Full Stack',
            'nivel' =>'Senior'
        ]);

        Profession::create([
            'titulo' => 'Desarrollador FrontEnd',
            'nivel' =>'Senior'
        ]);

        Profession::create([
            'titulo' => 'Desarrollador BackEend',
            'nivel' => 'Junior',
        ]);

        Profession::create([
            'titulo' => 'Product Owner',
            'nivel' => 'Mid',
        ]);

        Profession::create([
            'titulo' => 'Scrum Masterrrr',
            'nivel' => 'Senior',
        ]);

        Profession::create([
            'titulo' => 'Salesman',
            'nivel' => 'Senior',
        ]);

    }
}
````

3.2 - Luego de editado el archivo del seeder, es necesario registrar el seeders en el archivo __DatabaseSeeder.php__

```php
    public function run()
    {
        
        # hago la llamada al seeder
        $this->call(ProfessionSeeder::class);

    }
```

4 - Finalmente puedo correr las migraciones y los seeders con este comando:
ATENCION: El parámetro __fresh__ vaciará todas las tablas,  nunca hacer en producción
```php
php artisan migrate:fresh --seed
````




