![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)

# Capitulo 17 - Relaciones «Pertenece a»


El método __belongsTo__ permite trabajar con relaciones donde un registro pertenece a otro registro.  
Este método acepta como primer argumento el nombre de la clase que se quiera vincular.   
Eloquent determina el nombre de la llave foránea a partir del nombre del método (en este caso profession) y agregando el sufijo **_id** a este:

```php
public function profession()
{
    return $this->belongsTo(Profession::class);
}
````

Si en la base de datos el nombre de la llave foránea no sigue esta convención de __id__, es posible pasar el nombre de la columna como segundo argumento:

```php
public function profession()
{
    return $this->belongsTo(Profession::class, 'id_profession');
}
````

Por otro lado, si el modelo padre no usa una columna __id__ como su llave primaria o se quiere relacionar el modelo a una columna diferente, en ese caso de debera pasar un tercer argumento especificando el nombre de la columna que actuaría como llave del modelo padre:

```php
public function profession()
{
    return $this->belongsTo(Profession::class, 'profession_name', 'name');
}
````

En este caso Eloquent buscará la relación entre la columna __profession_name__ del modelo __Users__ y la columna __name__ del modelo __Profession__.

Hecho esto, utilizando cualquiera de las formas anteriores, se puede obtener la profesión del usuario:

```php
$user = User::first();
$user->profession;
````

## Relaciones Uno a Muchos con hasMany

Una relación uno a muchos es utilizada cuando un modelo puede tener muchos otros modelos relacionados.  
Por ejemplo, una profesión puede tener un número indeterminado de usuarios asociados a ésta.  
Dentro del modelo __Profession__ se puede decir que una __profesión__ tiene muchos __usuarios__:


```php
public function users()
{
    return $this->hasMany(User::class);
}
````


Ahora es posible entrar a la terminal de Thinker y obtener todos los usuarios de una profesión:

```php
$profession = Profession:first();
$profession->users;
````

Los métodos que permiten relacionar un modelo con muchos otros siempre van a retornar una colección, así esté vacía y los métodos que permiten relacionar un modelo con otro van a retornar el modelo o __null__.

## Construir Consultas
Es posible construir una consulta llamando al método de una relación.  

Por ejemplo, encadenando el método __where()__ a __users()__ es posible obtener todos los __usuarios__ asociados a una __profesión__ pero que tengan la columna __is_admin__ como __true__:

```php
$profession->users()->where('is_admin', true)->get();
````

[Siguiente Leccón -->](https://github.com/aledc7/Laravel/blob/master/lesson_18_Model_Factories.md)

