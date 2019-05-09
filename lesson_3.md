# Lesson 3

## Routes in Laravel

Begin to define our first routes, as we saw in the last chapter, we must edit the file /routes/web.php      
Note that if it were an API we should modify the file corresponding to the APIs, this is /route/api.php  

We could say that the routes are our  personalized "url addresses" that we define. The routes are defined as follows:    

```php

Route::get('/', function () {
    return view('welcome');
});


Route::get('/test',function (){
    return 'test of new route 1';
});

Route::get('/othertest',function (){
    return 'test of another route';
});

```

Above we have defined three different routes for our project, one will be the home page('/') and will return the welcome view     
Other one called test ('/test'), and will return a string 'test of another rute'
and third call othertest('/othertest') .  
In the cited example, these routes return a string of text, but the idea is to point to a component, which will be a section of our project.



```php
// route example that returns a parameter passed
Route::get('/usuarios/{id}', function($id){
    return "Mostrando detalle del usuario: {$id}";
});


// other example with a condition,
Route::get('/Hola/{name}/{nickname?}', function($name, $nickname = null){
    if ($nickname) {
        return "Welcome {$name}, your nickname is: {$nickname}";
    } else {
        return "Welcome {$name}, you don't set a nick name...";
    }
});
```






### Resources

The __resources__ folder containt all the assets, languages and views (blade templates)








