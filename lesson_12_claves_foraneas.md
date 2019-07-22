![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](h ttps://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)

# Capitulo 12 - Convenciones al ejecutar Migraciones


Al generar una migración con el comando __php artisan make:migration__ si utilizamos el siguiente formato para el nombre: create_NOMBRE DE LA TABLA_table, Laravel automáticamente generará el código requerido para crear dicha tabla:

```php
php artisan make:migration create_professions_table
````

## Producirá el siguiente código Boilerplate:
```php
public function up()
{
    Schema::create('professions', function (Blueprint $table) {
        $table->increments('id');
        $table->timestamps();
    });
}
````

## Ahora sólo tenemos que definir las columnas.

### También podemos pasar la opción __--create__ en caso de que no queramos usar la convención:

```php
php artisan make:migration new_professions_table --create=professions
````

## A la opción Create pasamos como Valor el nombre de la Tabla.

## Restricción de clave foránea
Podemos añadir una restricción de clave foranea a nuestro campo utilizando el método __foreign()__:
```php
Schema::create('users', function (Blueprint $table) {
    // ...
    $table->unsignedInteger('profession_id'); 
    $table->foreign('profession_id')->references('id')->on('professions');
    // ...
});
````

En este caso indicamos que el campo __profession_id__ va a hacer referencia al campo __id__ en la tabla __professions__.

Aquí es importante que el tipo del campo __profession_id__ coincida con el campo __id__ en la tabla __professions__. Es decir el campo __profession_id__ debe ser definido como un entero positivo, para ello usamos el método:
```php
$table->unsignedInteger('nombre_del_campo_aqui');
```
o también es posible:
```php
$table->integer('nombre_del_campo')->unsigned();
```

## Claves primarias  

Cuando diseñamos una Base de Datos, suele ser importante tener un campo (o combinación de campos) que pueda identificar de manera única a cada fila. Así como tienes un número de pasaporte que es único, cada usuario o profesión va a tener un identificador (id) único. En esta base de datos usaremos identificadores de tipo auto-incremento, es decir la primera fila obtendrá el identificador 1, la segunda 2, y así sucesivamente. Estos identificadores serán generados por el motor de la base de datos.

## Claves Foráneas
Para asociar una tabla con otra, vamos a utilizar una clave foránea. Por ejemplo en la tabla de usuarios, utilizaremos el campo __profession_id__, cuyo valor va a ser un identificador (__id__) válido de uno de los registros de la tabla de profesiones. De esta manera asociaremos un registro (fila) la tabla usuarios con un registro (fila) de la tabla de profesiones. En este tipo de relaciones solemos decir que un Usuario pertenece a una Profesión. También podemos decir que una Profesión tiene muchos Usuarios. Puesto que pueden existir 100 usuarios que sean desarrolladores Back-End o 50 usuarios que sean desarrolladores Front-End, cada profesión va a tener asignada muchos usuarios. Por otro lado cada usuario solo puede tener asignada una profesión (aunque en la vida real hay personas que tienen más de una profesión, en nuestro sistema esto no es relevante).   

## ¿Crear migraciones o modificar las ya existentes?
Para evitar que el número de migraciones crezca sin control, puedes modificar las migraciones ya existentes. Ten en cuenta que esto suele ser posible en etapas tempranas del desarrollo donde la Base de Datos no existe en producción y todos los datos son de prueba (no importa destruir y reconstruir la base de datos cada vez). Si luego de 3 meses de lanzar tu proyecto debes agregar un campo a la tabla de usuarios, en este caso te recomendaría crear una migración nueva. Porque así no solo podrás modificar la tabla de usuarios (sin eliminarla y recrearla) sino que además mantendrás un historial de los cambios que se han producido en diferentes versiones de tu aplicación.

## Cambié una migración y ya nada funciona…
En casos donde ejecutar __php artisan migrate:refresh__ y comandos similares, siempre produzca un error, puedes solucionarlo borrando todas las tablas de tu base de datos o ejecutando __php artisan migrate:fresh__.

Ten en cuenta que ejecutar php artisan migrate:fresh va a __eliminar todas las tablas__. Hazlo solamente si no te importa perder los datos (porque solo son de prueba, etc).


