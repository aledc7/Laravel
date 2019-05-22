![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

# Capitulo 9
## Introducción a Base de Datos - ORM


> Las bases de datos son uno de los aspectos más importantes de un proyecto. Sin embargo el proceso de tener que diseñar, crear y llevar el control de la misma puede resultar bastante tedioso. Afortunadamente Laravel nos proporciona un mecanismo llamado Migraciones con el cual podremos diseñar la estructura de nuestra base de datos y mantener su historial de cambios a lo largo del desarrollo del proyecto.
>  


En este capítulo veremos una introducción al manejo de base de datos con Laravel, se mostrará la consola interactiva de Laravel llamada __Tinker__ para probar el constructor de consultas del framework y su __ORM Eloquent__, crearemos nuestras primeras tablas, insertaremos datos, realizaremos consultas, veremos una introducción al manejo de relaciones entre tablas y registros con el framework.

## ORM  [Eloquent](https://laravel.com/docs/5.8/eloquent#introduction)

El ORM de Eloquent incluido con Laravel proporciona una implementación de ActiveRecord simple y hermosa para trabajar con su base de datos. Cada tabla de base de datos tiene un "Modelo" correspondiente que se utiliza para interactuar con esa tabla. Los modelos le permiten consultar datos en sus tablas, así como insertar nuevos registros en la tabla.





## Qué son las migraciones
Las migraciones son un mecanismo proporcionado por Laravel con el que podemos tener una especie de control de versiones sobre los cambios en la estructura de nuestra base de datos. Con las migraciones podemos diseñar esta estructura utilizando PHP y programación orientada a objetos, sin necesidad de escribir código SQL.

Las migraciones son agnósticas al motor de base de datos que tu proyecto use. Al crear un esquema, las migraciones crearán las tablas para el motor de base de datos que tengamos configurado, pero estas mismas migraciones las podemos usar con otros motores de bases de datos diferentes. Es decir, podemos usar el mismo esquema en múltiples motores de bases de datos (siempre que el motor sea soportado por Laravel).

Por defecto las migraciones se encuentran en el directorio database/migrations. Cada migración es un archivo .php que incluye en el nombre del archivo la fecha y la hora en que fue creada la migración (en formato timestamp) y el nombre de la migración.

Los motores de base de datos soportados por defecto son MySQL, PostgreSQL, SQLite y SQL Server. Además hay componentes de terceros que soportan otros tipos.

Migraciones por defecto
Al crear un nuevo proyecto, Laravel incluye por defecto dos migraciones:
```sql
2014_10_12_000000_create_users_table.php
2014_10_12_100000_create_password_resets_table.php
```
Una migración no es más que una clase de PHP que extiende de la clase Migration. El nombre de la clase corresponde al nombre del archivo, en el caso de 2014_10_12_000000_create_users_table.php, encontramos, pero con formato “studly case” (la primera letra de cada palabra en mayúscula, comenzando con mayúscula) en lugar de separado por guiones:

```php
class CreateUsersTable extends Migration
{
    // ...
}
```

## Métodos de una migración
Dentro de la clase de la migración encontramos dos métodos, up() y down():

```php
class CreateUsersTable extends Migration
{
    public function up()
    {
        // ... 
    }

    public function down()
    {
        // ...
    }
}
```


En el método up() vamos a especificar qué queremos que haga nuestra migración. Por decirlo de alguna forma, en qué manera queremos que “evolucione” nuestra base de datos cuando se ejecute dicha migración. Típicamente agregaremos tablas a la base de datos, pero también podemos agregar columnas a una tabla ya existente, o incluso podemos generar una migración para eliminar una tabla o columna que ya no necesitemos.

Para crear una tabla llamamos al método create del Facade Schema, pasando como primer argumento el nombre de la tabla que queremos crear (en este caso users) y como segundo argumento una función anónima que recibe como argumento un objeto de la clase Blueprint. Con los métodos que nos provee este objeto diseñaremos la estructura de la tabla:

```php
public function up()
{
    Schema::create('users', function (Blueprint $table) {
        $table->increments('id');
        $table->string('name');
        $table->string('email')->unique();
        $table->string('password');
        $table->rememberToken();
        $table->timestamps();
    });
    
```
Es una buena practica que el nombre del archivo de la migración coincida con lo que estamos haciendo dentro de la clase.

El método down() nos permite revertir o “devolver” la operación realizada en el método up(). En el caso de CreateUsersTable, down() nos permitirá eliminar la tabla users (utlizando el método dropIfExists del Facade Schema):

```php
public function down()
{
    Schema::dropIfExists('users');
}
```

Constructor de Esquemas (Schema Builder)
La clase Blueprint nos permite construir nuestras tablas con una interfaz orientada a objetos, a través de diferentes métodos, por ejemplo:
```php
$table->string('nombre_de_la_columna') permite crear una columna de tipo VARCHAR (cadena de texto).

$table->integer('nombre_de_la_columna') permite crear una columna de tipo INTEGER (entero).
```
Podemos encadenar métodos para especificar características adicionales, por ejemplo:
```php
$table->integer('nombre_de_la_columna')->unsigned()->default(0); crea una columna de tipo entero sin signo y cuyo valor por defecto será 0.
```
Puedes ver todos los métodos disponibles para crear columnas en la documentación oficial de Laravel.

## Métodos Helpers
Una de las tantas ventajas de usar el constructor de esquemas de Laravel, es que este incluye métodos helpers que facilitan tareas comunes y evitan la necesidad de duplicar código, por ejemplo el método: $table->timestamps(); agregará 2 columnas: created_at y updated_at de tipo timestamp (marca de tiempo) a la tabla en cuestión. Estas columnas son bastante típicas para almacenar el momento en que un registro fue creado o actualizado.

En programación un Helper no es más que una función de apoyo que permite resolver tareas genéricas / comunes.

# Configurar la base de datos

La configuración de la base de datos la hacemos en el archivo .env que está dentro del directorio principal de nuestro proyecto:
```sql
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=curso_styde
DB_USERNAME=root
DB_PASSWORD=root
```

En DB_DATABASE colocamos el nombre de nuestra base de datos, en DB_USERNAME el usuario de la base de datos y en DB_PASSWORD la contraseña de la base de datos.

Aprende más sobre la configuración de entorno en Laravel 5.*.

## Ejecutar las migraciones
Con el comando de Artisan migrate podemos ejecutar todas las migraciones:
```php
php artisan migrate
```
Al hacer esto Laravel automáticamente creará nuestras tablas en la base de datos. Al ejecutar nuestras migraciones por primera vez, Laravel creará una tabla llamada migrations donde va a guardar la información de las migraciones que han sido ejecutadas.

## Facades
En la lección mencioné que Schema y Route son Facades. En Laravel los Facades son una manera de acceder a las clases internas del framework con una interfaz estática y fácil de usar. Pero esto no quiere decir que Laravel esté conformado sólo de clases estáticas, al contrario, Laravel utiliza muy buenas practicas de la programación orientada a objetos como la inyección de dependencias, mientras que además incluye Facades y funciones helpers para que sea sencillo interactuar con las clases del framework.

Puedes aprender más en la lección Qué son los facades y cómo implementarlos en tu proyecto.

En la próxima lección aprenderemos a crear nuestras propias migraciones y a configurar la base de datos.

## Ejercicio
Crea una base de datos y configura el archivo .env para que puedas ejecutar las migraciones con el comando php artisan migrate.   
Una vez que lo logres verifica que las tablas se hayan creado con éxito utilizando cualquier administrador de bases de datos.

