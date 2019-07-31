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

[Lista con todos los métodos](https://laravel.com/docs/5.5/eloquent-collections#available-methods)



## Leer y asignar Atributos
Con Eloquent, todos los atributos de un modelo (o columnas de un registro) son tratados como si de propiedades públicas de la clase que se tratasen.   
Por ejemplo si quieres obtener el nombre de un usuario almacenado en la variable $user, se podrá escribir: __$user->name__; // obtiene el nombre del usuario.

Para re-asignar otro nombre se hace mediante __$user->name = 'Aledc'__;

Estos cambios no serán guardados automáticamente en la tabla de usuarios en la base de datos, sino que necesitas ejecutar el método save para hacer esto:

```php
$user->save(); //inserta o actualiza el usuario en la base de datos
````


Eloquent detectará si debe ejecutar un __INSERT__ o un __UPDATE__ dependiendo si el usuario existe o no, respectivamente.

La propiedad __exists__ de Eloquent, nos permite averiguar si un modelo existe o no, ejemplo: 
__$user->exists__ //devuelve __TRUE__ si el usuario ya existe en la base de datos, __FALSE__ de lo contrario.  


## Evitar fallos de seguridad por asignación masiva de datos

La excepción __MassAssignmentException__ es una forma en la que el ORM protege la aplicación.  
Una vulnerabilidad de asignación masiva ocurre cuando un usuario envía un parametro inesperado mediante una solicitud y dicho parametro realiza un cambio en la base de datos que no se esperaban.  
Por ejemplo, un usuario podría, utilizando Chrome Developer Tools o herramientas similares, agregar un campo oculto llamado __is_admin__ con el valor de __1__ y enviar la solicitud de registro de esta manera.   
Si no se tiene cuidado con esto entonces cualquier usuario podría convertirse en administrador de la aplicación, con consecuencias nefastas para tu sistema.  

Para evitar esto, dentro del modelo se debe agregar la propiedad __$fillable__ y asignarle como valor un array con las columnas que se quieren permitir que puedan ser cargadas de forma masiva:

```php
class User extends Model
{
    protected $fillable = ['name', 'password', 'email'];
}
````

También esa disponible la propiedad __$guarded__. Al igual que __$fillable__, esta propiedad tendrá como valor un array, pero en este caso las columnas que se indican son las que no se quiere que puedan ser cargadas de forma masiva:

```php
class User extends Model
{
    protected $guarded = ['is_admin'];
}
````

## Asignar un campo no fillable
Para asignar un valor a un campo que no está dentro de __$fillable__, se puede asignar una nueva instancia de un modelo en una variable y luego asignar el campo de forma manual:

```php
$user = new User(['name' => 'Alejandro DC', 'password' => bcrypt('Pirulo')]);

$user->is_admin = true;

$user->save();
````


Se puede observar que __new User($datos)__ solo crea un nuevo modelo sin persistirlo en la base de datos VS __User::create($datos)__ que crea un nuevo modelo y lo inserta en la base de datos en un solo paso.   

## Convertir atributos
La propiedad __$casts__ permite convertir atributos a diferentes tipos de datos dentro de un modelo. __$casts__ recibe como valor un array donde la llave es el nombre del atributo que será convertido y el valor el tipo al que lo se quiera convertir:

```php
protected $casts = [
    'is_admin' => 'boolean'
];
````

En este caso se convertirá la propiedad __is_admin__ a boolean.





[Ir a la siguiente lección --> Relaciones (Belong To)](https://github.com/aledc7/Laravel/blob/master/lesson_17_relaciones.md)
