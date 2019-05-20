![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

# Lesson 6 - Vistas

## Crear una vista
Las vistas generalmente se encuentran en el directorio 
```php
/resources/views 
```
de la carpeta principal de nuestro proyecto.  
Crear una vista con Laravel es muy sencillo, simplemente necesitamos crear un archivo .php en el directorio  /views. Dentro de este archivo escribimos el HTML de la vista.

### Retornar una vista
Para retornar una vista retornamos el llamado a la función helper view pasando como argumento el nombre de la vista.  
El nombre del archivo es relativo a la carpeta resources/views y no es necesario indicar la extensión del archivo:

```php
public function index()
{
    return view('users');
}
```

### Pasar datos a la vista: 
Podemos pasar datos a la vista mediante un arreglo asociativo, donde las llaves son el nombre de las variables que queremos pasar a la vista y el valor son los datos que queremos asociar:

```php
$users = [
    'Joel',
    'Ellie',
    'Tess',
    //...
];

return view('users', [
    'users' => $users
]);
```

También podemos usar el método with encadenándolo al llamado a la función view para pasar datos a la vista en formato de array asociativo:
```php
return view('users')->with([
    'users' => $users
]);
```

Con with también podemos pasar las variables de forma individual:

```php
return view('users')
    ->with('users', $users)
    ->with('title', 'Listado de usuarios');
```

Si los datos que queremos pasar a la vista se encuentran dentro de variables locales podemos utilizar la función compact,  la cual acepta como argumentos los nombres de las variables y las convierte en un array asociativo:

```php
$users = [
    ...
];

$title = 'Listado de usuarios';

return view('users', compact('users', 'title'));
```

### Escapar código HTML:
Laravel nos ofrece un helper llamado e que nos permite escapar HTML que podría ser insertado por los usuarios de nuestra aplicación, de manera de prevenir posibles ataques XSS:

```php
<li><?php echo e($user) ?></li>
```
