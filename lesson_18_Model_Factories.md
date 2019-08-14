![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)

# Model Factories
Con esta herramoienta podrénos generar una fabrica que basada en los modelos, generen insert en las tablas, con datos de tipo preciso.


# Generar un Model Factory

Para poder utilizar un Model Factory necesitamos generarlo primero con el comando make:factory. El Model Factory será generado en el directorio database/factories.

```php
php artisan make:factory ProfessionFactory
````

Dentro de nuestro Model Factory especificamos el atributo o los atributos que queremos generar de forma aleatoria:

```php
$factory->define(\App\Profession::class, function (Faker $faker) {
    return [
        'title' => $faker->sentence
    ];
});
````

Además es importante que indiquemos el nombre del modelo que queremos enlazar a dicho Model Factory (en este caso __\App\Profession::class__).

Para no tener que agregar el nombre del modelo de forma manual podemos pasar la opción __--model__ al comando __make:factory__

```php
php artisan make:factory ProfessionFactory --model=Profession
````

Al generar un modelo con el comando __make:model__ también podemos generar un Model Factory o una migración pasando las opciones -m o -f , o ambos juntos...   
por ejemplo:


```php
php artisan make:model Skill -mf
````

Utilizando el componente de __PHP Faker__ indicamos que el valor de 'titulo' será una oración aleatoria:

```php
$factory->define(\App\Profession::class, function (Faker $faker) {
    return [
        'titulo' => $faker->sentence
    ];
});
````

Pasando un número como primer argumento a __sentence__ se puede indicar el número de palabras que tendrá la oración: __$faker->sentence(3)__.  
Esto devolverá oraciones de 2, 3 o 4 palabras, o si se quiere especificamente que sean solo 3 palabras se puede llamar al método de esta forma: __$faker->sentence(3, false)__  .
El agregado de __false__ le sacará la aleatoriedad de hacer las oraciones con 1,2 o 3.   



## Componente Faker
El componente Faker es una librería de PHP que genera datos de prueba aleatoriamente y de forma coherente (mas o menos).     
Por ejemplo, podemos generar un nombre:

```php
$faker->name;
// 'Jazmyne Romaguera'
````

Asi se generaria un texto aleatorio:

```php
$faker->text;
// Dolores sit sint laboriosam dolorem culpa et autem. Beatae nam sunt fugit
// et sit et mollitia sed.
````
2

Incluso números de teléfono:

```php
$faker->cellphone;
// 9432-5656
````


#  Utilizar un Model Factory
Para utilizar un Model Factory se debe llamar al __helper factory__, especificando el nombre del modelo como argumento y finalmente encadenando el llamado al método __create__.

```php
factoy(User::class())->create();
````

Esto va a crear un usuario en la Base de datos y va a retornar el modelo en cuestión, en este caso un objeto de la clase User:

```php
App\User {
    name: "Alejandro De Castro",
    email: "aledc@example.com",
    updated_at: "2019-08-14 09:52:15",
    created_at: "2019-08-14 09:52:15",
    id: 7,
````


Cada vez que se ejecute el método __create()__ se creará un nuevo registro aleatorio.  
 Si se pasa un array asociativo al método create() se puede sobrescribir o asignar propiedades extras:

```php
factory(User::class)->create([
    'profession_id' => $professionId
]);
````

Para cargar un determinado número de registros se debe pasar como segundo argumento la cantidad que se quiera crear:

```php
factory(User::class, 48)->create();
````


También es posible lograr esto utilizando el método __times()__:

```php
factory(User::class)->times(48)->create();
````


Aquí encontrarás toda la documentación del Faker :point_down:   

()[https://github.com/fzaninotto/Faker]  





