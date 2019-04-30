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

Above we have defined three different routes, one will be the home page('/') .   
Another one called test ('/test'), and a third call othertest('/othertest') .  
In the cited example, these routes return a string of text, but the idea is to point to a component, which will be a section of our App.    




