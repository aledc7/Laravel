![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)

# Capitulo 16 - Usando Eloquent ORM de forma Interactiva con Tinker

Laravel cuenta con una consola dinámica llamada __Tinker__.   
Esta terminal permite usar métodos de Eloquent para interactuar con los modelos.


## Acceder a Tinker
Para acceder al entorno con el comando de Artisan tinker:  

```php
php artisan tinker
````

Desde el entorno interactivo se puede ejecutar expresiones de PHP y también es posible acceder a las clases del proyecto y del framework.

## Retornar todos los registros con el método __all()__

Utilizando el método __all()__ retornamos todos los registros asociados a un modelo:

```php
$professions = Profession::all();
````


Los métodos __all()__ y __get()__ en Eloquent retornan colecciones (objetos de la clase Illuminate\Database\Eloquent\Collection) las cuales «envuelven» el array de resultados y proveen de funciones y métodos adicionales, por ejemplo:

Es posible obtener el primer resultado utilizando el método __first()__ y el último utilizando el método __last()__.   
También es posible obtener un resultado aleatorio utilizando el método __random()__:

```php
$profession->first(); // Obtenemos el primer resultado

$profession->last(); // Obtenemos el último resultado

$profession->random(1); // Obtenemos un resultado aleatorio
````

Estos métodos de la clase Collection no generan nuevas consultas SQL sino que operan sobre los resultados ya encontrados.

## Seleccionar un campo con el método __pluck()__   
Utilizando el método __pluck()__ es posible retornar una nueva colección que contenga un listado de un solo campo en vez de un listado de objetos.   
Por ejemplo, es posible obtener solo el campo title de la siguiente forma:

```php
$professions->pluck('title');
````

Esto devolverá:    
```php
=> Illuminate\Support\Collection {#736
     all: [
       "Desarrollador back-end",
       "Desarrollador front-end",
       "Diseñador web",
     ],
   }
````


## Crear una Colección de forma Manual

Con el helper collect es posible crear una colección de forma manual:

```php
collect(['Aledc', 'aledc.com', 'Laravel']);
````


Sin embargo éste será un objeto de la clase Illuminate\Support\Collection que representa la colección «base» en vez de Illuminate\Database\Eloquent\Collection.

Las colecciones de Eloquent extienden de la colección base pero proveen algunos métodos extra asociados al ORM.

## Declarar métodos en el modelo
Se puede declarar métodos dentro de un modelo y utilizarlos cuando se interactúa con los objetos de estos modelos:

# Declarando métodos no estáticos:
Asociamos un método para ser utilizado sobre un objeto (que representa un registro de la base de datos):

```php
public function isAdmin()
{
    return $this->email === 'info@aledc.com';
}
````

En este caso __isAdmin()__ devuelve true si el email del usuario es igual al valor con el que está siendo comparado:

```php
$user = User::find(1);
$user->isAdmin(); // Devuelve true o false
````

## Declarando métodos estáticos:
Asociamos un método a la clase del modelo como tal, la cual representa el acceso a una tabla de la base de datos. Estos métodos son usados típicamente para consultas:

```php
public static function findByEmail($email)
{
    return static::where('email', $email)->first();
}
````

Luego se podrá usar __User::findbyEmail('info@aledc.com') para buscar a un usuario por el campo email y obtener un objeto User como resultado (o null si no se encuentra ningún registro).



[Ir a la siguiente lección -->]()
