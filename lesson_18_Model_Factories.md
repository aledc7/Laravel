![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)

# Model Factories


Notas
Generar un Model Factory
Para poder utilizar un Model Factory necesitamos generarlo primero con el comando make:factory. El Model Factory será generado en el directorio database/factories.

php artisan make:factory ProfessionFactory
1
php artisan make:factory ProfessionFactory
Dentro de nuestro Model Factory especificamos el atributo o los atributos que queremos generar de forma aleatoria:

$factory->define(\App\Profession::class, function (Faker $faker) {
    return [
        'title' => $faker->sentence
    ];
});
1
2
3
4
5
$factory->define(\App\Profession::class, function (Faker $faker) {
    return [
        'title' => $faker->sentence
    ];
});
Además es importante que indiquemos el nombre del modelo que queremos enlazar a dicho Model Factory (en este caso \App\Profession::class).

Para no tener que agregar el nombre del modelo de forma manual podemos pasar la opción --model al comando make:factory:

php artisan make:factory ProfessionFactory --model=Profession
1
php artisan make:factory ProfessionFactory --model=Profession
Al generar un modelo con el comando make:model también podemos generar un Model Factory y/o una migración pasando las opciones -f y/o -m por ejemplo:

php artisan make:model Skill -mf
1
php artisan make:model Skill -mf
Utilizando el componente de PHP Faker indicamos que el valor de title será una oración aleatoria:

$factory->define(\App\Profession::class, function (Faker $faker) {
    return [
        'title' => $faker->sentence
    ];
});
1
2
3
4
5
$factory->define(\App\Profession::class, function (Faker $faker) {
    return [
        'title' => $faker->sentence
    ];
});
Pasando un número como primer argumento a sentence podemos indicar el número de palabras que queremos que contenga la oración: $faker->sentence(3). Esto devolverá oraciones con 2, 3 o 4 palabras, o si queremos estrictamente que sean 3 palabras podemos llamar al método de esta forma: $faker->sentence(3, false)

Componente Faker
El componente Faker es una librería de PHP que genera datos de prueba por nosotros. Por ejemplo, podemos generar un nombre:

$faker->name;
// 'Jazmyne Romaguera'
1
2
$faker->name;
// 'Jazmyne Romaguera'
O un texto aleatorio:

$faker->text;
// Dolores sit sint laboriosam dolorem culpa et autem. Beatae nam sunt fugit
// et sit et mollitia sed.
1
2
3
$faker->text;
// Dolores sit sint laboriosam dolorem culpa et autem. Beatae nam sunt fugit
// et sit et mollitia sed.
Incluso números de teléfono:

$faker->cellphone;
// 9432-5656
1
2
$faker->cellphone;
// 9432-5656
Utilizar un Model Factory
Para utilizar un Model Factory debemos llamar al helper factory, especificando el nombre del modelo como argumento y finalmente encadenando el llamado al método create.

factory(User::class())->create();
1
factory(User::class())->create();
Esto va a crear un usuario en la base de datos y va a retornar el modelo en cuestión, en este caso un objeto de la clase User:

App\User {
    name: "Jazmyne Romaguera",
    email: "ciara.willms@example.com",
    updated_at: "2017-11-24 15:55:32",
    created_at: "2017-11-24 15:55:32",
    id: 4,
}
1
2
3
4
5
6
7
App\User {
    name: "Jazmyne Romaguera",
    email: "ciara.willms@example.com",
    updated_at: "2017-11-24 15:55:32",
    created_at: "2017-11-24 15:55:32",
    id: 4,
}
Cada vez que ejecutamos el método create() creamos un nuevo registro aleatorio. Si pasamos un array asociativo al método create() podemos sobrescribir o asignar propiedades extras:

factory(User::class)->create([
    'profession_id' => $professionId
]);
1
2
3
factory(User::class)->create([
    'profession_id' => $professionId
]);
Para cargar un determinado número de registros pasamos como segundo argumento la cantidad que queremos crear:

factory(User::class, 48)->create();
1
factory(User::class, 48)->create();
También podemos lograrlo utilizando el método times():

factory(User::class)->times(48)->create();
1
factory(User::class)->times(48)->create();


Material relacionado
(Documentación del componente Faker)[https://github.com/fzaninotto/Faker]




