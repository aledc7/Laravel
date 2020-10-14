![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)


# Lesson 7 - Plantillas con Blade

En esta lección comenzaremos a aprender sobre Blade, el sistema de plantillas de Laravel, el cual nos provee de muchas características que deberíamos tener en un lenguaje de plantillas, como por ejemplo la capacidad de escapar los datos de forma automática.

Para utilizar el sistema de plantillas de Laravel debemos renombrar nuestras vistas para que tengan la extensión .blade.php


Si queremos imprimir una variable, podemos hacerlo utilizando la sintaxis de dobles llaves {{ }}

```php
<li>{{ $user }}</li>
```

### Ciclos y estructuras
Si queremos utilizar ciclos y estructuras condicionales, podemos utilizar directivas. Las directivas de Blade van precedidas por un arroba (@) y luego el nombre de la directiva:

```php
@foreach ($users as $user)
    <li>{{ $user }}</li>
@endforeach
```

### También podemos utilizar la directiva @if:
```php
@if (! empty($users))
    ...
@endif
```

La directiva @if puede ser utilizada junto con un bloque else (utilizando @else):
```php
@if (! empty($users))
    ...
@else
    <p>No hay usuarios registrados.</p>
@endif
```

Podemos utilizar la directiva @elseif, que como su nombre sugiere nos permite utilizar un bloque else if:
```php
@if (! empty($users))
    ...
@elseif ($users < 3)
    <p>Hay menos de 3 usuarios registrados.</p>
@else
    <p>No hay usuarios registrados.</p>
@endif
```

Blade también tiene la directiva @unless, que funciona como un condicional inverso:
```php
@unless (empty($users))
    <ul>
        @foreach ($users as $user)
            <li>{{ $user }}</li>
        @endforeach
    </ul>
@else
    <p>No hay usuarios registrados.</p>
@endunless
```

En el ejemplo anterior queremos mostrar el listado de usuarios a no ser que la lista esté vacia. De lo contrario queremos mostrar el mensaje del bloque else.

También podemos utilizar la directiva @empty que es una forma más corta de escribir @if (empty (...))
```php
@empty($users)
    <p>No hay usuarios registrados.</p>
@else
    <ul>
        @foreach ($users as $user)
            <li>{{ $user }}</li>
        @endforeach
    </ul>
@endempty
```

En este caso por supuesto invertimos los bloques dentro del condicional.

Además de @foreach, también podemos utilizar @for:
```php
@for ($i = 0; $i < 10; $i++)
    El valor actual es {{ $i }}
@endfor
```

Con la directiva @forelse podemos asignar una opción por defecto a un ciclo foreach sin utilizar bloques anidados:
```php
@forelse ($users as $user)
    <li>{{ $user }}</li>
@empty
    <li>No hay usuarios registrados.</li>
@endforelse
```

Cuando utilizamos la sintaxis de dobles llaves Blade nos protege automáticamente de ataques XSS.

Vistas en caché
Laravel compila y guarda en caché nuestras vistas, por lo que usar el motor de plantillas Blade no afecta el rendimiento de nuestra aplicación. Puedes ver estas vistas compiladas en el directorio /storage/framework/views.

Utilizando el comando view:clear podemos eliminar desde la terminal la vistas en caché:

```php
php artisan view:clearv
```

[ir a la lección 8 --> Layouts](https://github.com/aledc7/Laravel/blob/master/lesson_8_Layouts.md)
