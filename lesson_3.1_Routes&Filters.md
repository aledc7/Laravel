![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)
# Lesson 3.1 - RUTAS CON FILTROS




Es posible agregar __Filtros__ a todas las rutas que permitan verificar la información que el usuario está enviando a través de la URL para redirigirlo a la acción correcta, sin necesidad de utilizar middleware, validaciones u otro tipo de estructura de control, tan solo haciendo uso del método __where()__ dentro el archivo de rutas.

El componente de rutas de Laravel permite capturar las peticiones realizadas por un usuario, de tal manera que podamos evaluar los parámetros enviados y devolver la respuesta acorde a cada petición.  
Cuando iniciamos un nuevo Proyecto, las rutas se establecen por defecto dentro del archivo __routes.php__ en el directorio app\Http, en este archivo podrás ver algo similar a esto:

```php
Route::get('/', function () {
    return view('welcome');
});
```


Esta es una ruta de tipo GET que devuelve una __Vista__ llamada __welcome__ (recuerda que en Laravel las vistas se almacenan en el directorio resources/views, utilizando la extensión .blade.php).

```php
Route::get('user', function () {
    return "foo";
});

Route::post('user', function () {
    return "bar";
});
```


En este caso tenemos dos rutas hacia el mismo url __/user__, diferenciadas por el tipo de petición, una es de tipo __POST__ (para enviar datos desde un formulario) y otra de tipo __GET__, de esta forma usando un tipo diferente indicamos cual de ellas debe encargarse de cada solicitud. Por ejemplo, una para mostrar el formulario de registro de usuarios y la otra para almacenar los datos que este formulario envié.  

En las rutas también podemos recibir parámetros cómo:

```php
Route::get('user/{id}', function ($id) {
    return $id;
});
 
Route::get('user/{id}', function ($id) {
    return $id;
});
```

En este caso vamos a recibir por medio de la variable $id, por ejemplo, el identificador de un usuario en la base de datos, ahora, si quisieramos recibir como parámetro en otra ruta el nombre del usuario pero usando el mismo slug:

```php
Route::get('user/{id}', function ($id) {
    return $id;
});

Route::get('user/{name}', function ($name) {
    return $name;
});
```


Si tenemos dos rutas del mismo tipo, con el mismo slug (/user/) y recibiendo los parámetros bajo la misma estructura, podemos enfrentarnos a un problema, ya que probablemente todas las peticiones de tipo /user/nombre, /user/10 van a ser capturadas por la misma ruta, en este caso la primera en orden descendente.

Para evitar este tipo de errores se debe hacer uso de los __FILTROS__



# Filtros en Rutas

Los filtros se pueden utilizar haciendo uso de expresiones regulares, por ejemplo, queremos que en el primer bloque capture solo las variables de tipo numérico ($id).

```php
Route::get('user/{id}', function ($id) {
    return $id;
})->where(['id' => '[\d]+']);

Route::get('user/{name}', function ($name) {
    return $name;
});
````

Entonces desde ahora todas las rutas de tipo /user/10, van a ser capturadas por la primera ruta. 
Y en el caso de cuando el tipo de dato es del tipo __string__, podemos hacer algo similar para los string:

```php

Route::get('user/{id}', function ($id) {
    return $id;
})->where(['id' => '[\d]+']);

# ACA HACEMOS LO MISMO CON EL WHERE PERO PARA LOS STRINGS
Route::get('user/{name}', function ($name) {
    return $name;
})->where(['name' => '[-\w]+']);
```


También es posible limitar a una lista específica de valores posibles.  
Supongamos que además del problema anterior de usar user/{name} y user/{id}, tu sistema debe ser capaz de manejar posibles peticiones de tipo __user/create__, __user/delete__, __user/update__.

Si se ingresa cualquiera de estas URL, la petición va a ser capturada por la segunda ruta, pero eso no es lo que se desea, ya que se convertirá en una simple cadena de texto en vez de disparar las acciones de CRUD.

Esto puede resolverse de la siguiente manera:

```php
Route::get('user/{id}', function ($id) {
    return $id;
})->where(['id' => '[\d]+']);

Route::get('user/{slug}', function ($slug) { 
    return $slug; 
})->where(['slug' => 'create|delete|update']);

Route::get('user/{name}', function ($name) {
    return $name;
})->where(['name' => '[-\w]+']);
````

En el caso de arriba, y recordando el tema de precedencias, debemos asegurarnos que la ruta que posee la restricción este primera, para que pueda ser evaluada correctamente.

Desde ahora cuando se trate de __user/delete__, __user/update__, __user/create__, las peticiones van a ser capturadas por este segundo bloque ya que hemos definido explícitamente cuáles son los posibles valores de ese __{slug}__ y cualquier valor de tipo string que esté fuera de esa lista va a ser capturado por la __tercera ruta__.  

Acá el bloque de código con las 3 rutas, en el orden correcto:

```php
Route::get('user/{id}', function ($id) {
    return "Esto es un ID:".$id;
})->where(['id' => '[\d]+']);

Route::get('user/{slug}', function ($slug) {
    return "Esto es un Slug:".$slug;
})->where(['slug' => 'create|delete|update']);

Route::get('user/{name}', function ($name) {
    return "Esto es un Nombre:".$name;
})->where(['name' => '[-\w]+']);
````
 
 
 
 A practicar -->
