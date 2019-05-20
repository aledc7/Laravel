![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

# Lesson 8 - Layouts con Blade

A medida que nuestro proyecto crece nuestras plantillas se vuelven más complejas y es inevitable encontrarnos con que estamos repitiendo etiquetas y estructuras que podríamos compartir entre multiples vistas. Es por ello que en esta lección te enseñaremos a integrar cualquier diseño usando Laravel Blade; para que de esta manera puedas sacarle provecho a las diferentes directivas que ofrece este motor de plantillas y evitar así la repetición de código HTML, además de mantener tus vistas sencillas, expresivas, elegantes y bien estructuradas.


Cuando trabajas con Laravel puedes utilizar Bootstrap, Materialize, Bulma o cualquier framework de CSS o diseño personalizado que quieras.

### Directiva @include
Blade incluye una directiva llamada @include. Para usarla solamente tenemos que pasarle el nombre del archivo que queremos incluir:

```php
@include('header')
    <h1>{{ $title }}</h1>
    ...
@include('footer')
```

Podemos usar múltiples directivas @include dentro de una misma plantilla de Blade.

### Helper asset()
El helper asset nos dará la ruta absoluta al archivo indicado:
```php
<link href="{{ asset('css/style.css') }}" rel="stylesheet">
```
Utilizando este helper podemos evitar que la ruta del archivo cambie dependiendo de la URL.

### Layout principal
En lugar de separar nuestra plantilla en diferentes archivos, podemos crear una sola plantilla que tendrá toda la estructura de nuestro diseño. Podemos llamar a esta plantilla layout.blade.php, por ejemplo, y colocar todo el código de nuestro diseño allí.

Utilizando la directiva @yield dentro de esta plantilla podemos indicar secciones (pasando como argumento el nombre de la sección) y luego en plantillas individuales podemos colocar el contenido de dichas secciones:
```php
<main role="main" class="container">
    @yield('content')
</main>
```

Puedes nombrar tu layout de cualquier forma, siempre y cuando coloques la extensión .blade.php. En este ejemplo se llama layout.blade.php.

Puedes agregar tantas directivas @yield como quieras a tu layout. Por ejemplo, puedes agregar una directiva yield para personalizar el título de la página:
```php
<title>@yield('title') - Styde.net</title>
```

### Extender de una plantilla
En cada una de nuestras plantillas individuales en lugar de incluir el header o footer le indicamos a Laravel que la vista debe extender de layout.blade.php. No es necesario colocar la extensión del archivo. Tampoco es necesario colocar la ruta completa, ya que Laravel por defecto buscará el archivo dentro del directorio resources/views:
```php
@extends('layout')
```

Hecho esto, debemos definir las secciones. Para ello utilizamos la directiva @section, pasando como argumento el nombre de la sección:

@section('title') Usuario {{ $id }} @endsection
```php
@section('content')
    <!-- Contenido de la sección -->
@endsection
```

Indicamos el final o cierre de la sección con la directiva @endsection.

La directiva @section define una sección de contenido, mientras que la directiva @yield es usada para mostrar el contenido de una sección específica.

Dado que el titulo es una sola línea, podemos pasar el contenido como el segundo argumento de @section:
```php
@section('title', "Usuario {$id}")
```
El código que se encuentra entre comillas es PHP y no Blade, por lo que en lugar de utilizar la sintaxis de dobles llaves {{ $id }} utilizaremos {$id} o simplemente $id.
