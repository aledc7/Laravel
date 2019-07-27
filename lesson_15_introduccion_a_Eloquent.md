![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)

# Capitulo 15 - Introducción a Eloquent


## Generar un modelo
Los modelos los podemos generar desde la consola utilizando el comando __make:model__ de Artisan:

```php
php artisan make:model Profession
````

La convención es nombrar al modelo con su primera letra en mayúsculas y escribirlo en singular (por ejemplo: «Profession» en vez de «professions»). Si queremos agregar dos palabras en el nombre, la convención es colocar la primera letra de cada palabra en mayúscula:

```php
php artisan make:model ProfessionCategory
````

Este formato es conocido como «Studly Caps».


Por defecto los modelos serán generados en el directorio __app__ de nuestra aplicación, con su nombre más la extensión .php. En nuestra caso Profession.php o ProfessionCategory.php.   
También podemos generar el modelo dentro de otro directorio, especificando el nombre del directorio antes del modelo:
```php
php artisan make:model Models/Profession
````

En este caso el modelo __Profession.php__ se encontrará en __app/Models/__. De no existir el directorio especificado, Laravel lo va a crear por nosotros.

## Especificar la tabla relacionada al modelo manualmente
Al utilizar un modelo no es obligatorio especificar el nombre de la tabla si seguimos las convenciones de Eloquent.  
Por ejemplo si utilizamos como nombre de modelo __Profession__ Laravel hará las consultas a la tabla __professions__.  
Si utilizamos __User__ Laravel hará las consultas a la tabla __users__.  
Por último si nuestro modelo fuese __ProfessionCategory__ la tabla por defecto sería __profession_categories__.

En caso de que el nombre de la tabla no sea igual al nombre del modelo, debemos especificarlo en el modelo definiendo la propiedad __protected $table__:

```php
class Profession extends Model
{
    protected $table = 'my_professions';
}
````

## Insertar datos utilizando un modelo
Podemos insertar datos llamando al método create del modelo:

```php
\App\Profession::create([
    'title' => 'Desarrollador back-end',
]);
````

Importando el modelo al principio del archivo, evitamos tener que hacer referencia a __\App\Profession__ cada vez que trabajemos con el modelo:

```php
use App\Profession;
````
Luego de importar el modelo, podemos hacerle referencia directamente:

```php
Profession::create([
    'title' => 'Desarrollador back-end',
]);
````

## Eliminar campos timestamps
Al insertar datos utilizando un modelo, Eloquent se encargará de cargar los valores de los campos __created_at__ y __updated_at__ de la tabla.  
Si no queremos utilizar estos campos, dentro de nuestro modelo debemos agregar la propiedad pública __$timestamps__ y darle el valor de __false__:

```php
public $timestamps = false;
````

## Realizar consultas
Podemos utilizar los modelos para hacer consultas a la base de datos. Utilizando el método __all()__ obtenemos todo el contenido de la tabla:

```php
$professions = Profession::all();
````

También podemos encadenar métodos del constructor de consultas para obtener resultados más específicos:
```php
$professionId = Profession::where('title', 'desarrollador back-end')->value('id');
````

Podemos retonar un resultado dependiendo de su __id__ mediante el método __find()__:

```php
$profession = Profession::find(1);
````



[Ir a la siguiente lecciion -->]()
