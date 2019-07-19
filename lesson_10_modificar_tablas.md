![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)

# Capitulo 10
## Modificando Tablas


Modificar una tabla
Para agregar una columna a una tabla, modificamos la migración ya existente. En nuestro caso añadimos un campo profession con un limite de 100 caracteres a la tabla users:
```php
<?php
//...
Schema::create('users', function (Blueprint $table) {
    $table->increments('id');
    $table->string('name');
    $table->string('email')->unique();  
    $table->string('profession', 100)->nullable(); // Agregamos una nueva columna
    $table->string('password');
    $table->rememberToken();
    $table->timestamps();
});
```

Encadenando nullable() indicamos que la columna puede contener valores nulos, es decir que su valor es opcional.

Comando reset
El comando de Artisan migrate:reset va a hacer un retroceso (roll back) de las migraciones ejecutadas previamente. En nuestro ejemplo va a eliminar las tablas users y password_resets.

```php
#
php artisan migrate:reset
```
Luego de ejecutar este comando, podemos ejecutar el comando php artisan migrate para volver a crear las tablas.

Modificar un campo ya existente
Para modificar un campo ya existente, por ejemplo el limite de caracteres del campo profession, agregamos el nuevo valor en la migración:

```php
<?php
//...
Schema::create('users', function (Blueprint $table) {
    // ...
    $table->string('profession', 50)->nullable(); // Cambiamos el límite de 100 a 50
    // ...
});
```

Para que estos cambios tengan efecto debemos ejecutar el comando de Artisan migrate:refresh.

Comando migrate:refresh
El comando migrate:refresh primero va a ejecutar un reset de todas las migraciones (llamando al método down()) y luego volverá a ejecutar las migraciones (llamando al método up()):

```php
#
php artisan migrate:refresh
```

## Modificar migraciones
Al realizar una modificación en la migración original tenemos el problema de que los comandos reset y refresh eliminaran el contenido de las tablas en la base de datos. Para evitar esto podemos (utilizando el comando make:migration) crear una nueva migración y agregar desde ahí las modificaciones que necesitamos:

```php
#
php artisan make:migration add_profession_to_users
```

Es una buena practica que el nombre de la migración sea descriptivo y haga referencia a lo que vamos a hacer. En este caso add_profession_to_users indica que queremos agregar el campo profession a la tabla users.

Dentro del método up() de la migración en lugar de usar el método create del facade Schema utilizaremos table y pasaremos como primer argumento el nombre de la tabla que queremos modificar:

```php
<?php
//...
Schema::table('users', (Blueprint $table)) {
    $table->string('profession', 50)->nullable();
});
```

Dentro de la función indicamos el campo que queremos agregar.

En el método down() especificamos la acción inversa, en este caso eliminar la columna que agregamos en el método up():

```php
<?php
//...
Schema::table('users', function (Blueprint $table) {
    $table->dropColumn('profession');
});
```

Con el método dropColumn eliminamos de la tabla la columna especificada como argumento.

Indicar la posición de una columna
Al modificar una migración utilizando Schema::table y agregar una columna, esta se va añadir al final de la tabla. Podemos lograr que la columna se cree en la posición que indiquemos utilizando el método after:

```php
<?php
//...
Schema::table('users', function (Blueprint $table)) {
    $table->string('profession', 50)->nullable()->after('password');
});
```

En este caso indicamos que queremos agregar la columna profession después del campo password.

## Comando rollback
Utilizando el camando de Artisan migrate:rollback Laravel regresará el último lote de migraciones ejecutado:

```php
#
php artisan migrate:rollback
```





