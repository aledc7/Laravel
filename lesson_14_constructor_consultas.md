![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)

# Capitulo 14 - Constructor de Consultas y Debbug


## Método insert
Con el método __DB::insert__ se puede escribir consultas SQL de forma manual para insertar contenido dentro de nuestra base de datos. Por ejemplo, el código del seeder ProfessionSeeder.php que utiliza el método DB::table:

```php
DB::table('professions')->insert([
 'title' => 'Desarrollador back-end',
]);
````

Puede ser re-escrito utilizando el método __DB::insert__, directamente con código SQL:
```php
DB::insert('INSERT INTO professions (title) VALUES ("Desarrollador back-end")');

````

Aunque __DB::insert__ nos da el mismo resultado que __DB::table__, cuando realizamos consultas de este tipo y recibimos datos ingresados por un usuario debemos tener mucho cuidado, ya que nos exponemos a ataques de inyección de SQL.

## Debuggeando con dd()

dd() es una función que permite debuggear para ver el contenido de variables,  se le pasa adentro el objeto que se quera visualizar

```php
db($nombre_Variable);
````
El código de arriba, al ejecutar __php artisan db:seed__  mostrará en terminal el contenido de la variable pasada.

Primero guardo en una variable una consulta a la base de datos:
```php
$professions1 = DB::select('SELECT id FROM professions where titulo = "Desarrollador BackEend"');
````

Luego utilizando db($variable) voy a debuggear para ver lo que haya dentro

```php
db($professions1);
````

la respuesta debería ser como la siguiente:

```php
 php artisan db:seed
Seeding: ProfessionSeeder
Seeding: UserSeeder
array:1 [
  0 => {#569
    +"id": 3
  }
]
````


## Inyección de SQL
Nuestro código es vulnerable a inyecciones de SQL cuando insertamos variables dentro de una solicitud de SQL. Por ejemplo, si tuvieras una consulta donde seleccionas una serie de articulos dependiendo de un autor:
```php
$sql = "SELECT * FROM articles WHERE id_author = $id";
````

Esta consulta trae todos los artículos escritos por un determinado autor. Sin embargo, dentro de esta consulta estamos insertando contenido de forma directa al colocar la variable $id.

Supón que ingresamos a los artículos del autor desde una URL, pasando como argumento el id del autor (?id=1 o articulos/{id}), en este caso retornaríamos todos los artículos escritos por el autor cuyo id sea igual a 1. Sin embargo, como nuestro código es vulnerable, un usuario malintencionado podría cambiar la URL por ?id=1 UNION SELECT password FROM users. La consulta realmente se estaría realizando de esta forma:
```php
SELECT * FROM articles WHERE id_author = 1 UNION SELECT password FROM users;
````

Esta consulta selecciona todos los artículos y luego selecciona todas las contraseñas almacenadas en la tabla users.

Al almacenar contraseñas en una base de datos asegurate de siempre encriptarlas.

## Parametros Dinámicos

Para evitar ataques de inyección de SQL podemos utilizar parámetros dinámicos.  
Laravel utiliza internamente el componente PDO de PHP y debido a esto podemos colocar marcadores en nuestra consulta.  
Laravel nos permite pasar sus valores en un array como segundo argumento del método:
```php
DB::insert('INSERT INTO professions (title) VALUES (?)', ['Desarrollador back-end']);
````

Otra forma de pasar los parametros es usando como marcador un parametro de sustitución con nombre y como segundo argumento pasamos un array asociativo de los respectivos parámetros con sus valores:

```php
DB::insert('INSERT INTO professions (title) VALUES (:title)', ['title' => 'Desarrollador back-end']);
````

Al hacer esto estaremos protegidos de ataques de inyección de SQL puesto que los parámetros dinámicos serán escapados de forma automática y segura.

## Método Select
Utilizando el método __DB::select__ podemos construir una consulta __SELECT__ de SQL de forma manual:

```php
DB::select('SELECT id FROM professions WHERE title = ?', ['Desarrollador back-end']);
````

Por otro lado, utilizando el constructor de consultas podemos realizar una consulta SQL de tipo SELECT, de la siguiente forma:

```php
$professions = DB::table('professions')->select('id')->take(1)->get();
````

El resultado de esta consulta es un objeto de la clase __Illuminate\Support\Collection__ que encapsula el array de datos y esto nos trae algunas ventajas extras: una interfaz orientada a objetos con la cual trabajar y muchas funciones extras. Por ejemplo, podemos utilizar el método first para obtener el primer resultado de la consulta (en el caso de este ejemplo, la primera profesión):

```php
$professions->first(); // en vez de $professions[0]
````

## Consultas con Condicionales (WHERE)
El constructor de consultas también nos permite realizar consultas condicionales utilizando where:

```php
$profession = DB::table('professions')->select('id')->where('title', '=', 'Desarrollador back-end')->first();
````

El operador __=__ dentro del método __where__ es opcional. Pasando el nombre de la columna junto con el valor, Laravel asumirá que quieres usar el operador de comparación de igualdad (=):

```php
where('title', 'Desarrollador back-end')
````


El método where también acepta un array asociativo, donde indicamos el nombre de la columna y el valor que esperamos encontrar:

```php
where(['title' => 'Desarrollador back-end'])
````


## Métodos Dinámicos
También podemos utilizar métodos dinámicos:

```php
$profession = DB::table('professions')->whereTitle('Desarrollador back-end')->first();
````

En este caso whereTitle es lo equivalente a escribir where('title', '=', 'Desarrollador back-end').

## Omitir el método Select de DB::table
Omitiendo el método select al utilizar __DB::table__ podemos retornar todas las columnas:

```php
$profession = DB::table('professions')->where('title', '=', 'Desarrollador back-end')->first();
````

[Ir al siguiente capìtulo -->]()
