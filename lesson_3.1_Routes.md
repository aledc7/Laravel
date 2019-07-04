![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)
# Lesson 3.1 - RUTAS CON FILTROS




Es posible agregar __Filtros__ a todas las rutas que permitan verificar la información que el usuario está enviando a través de la URL para redirigirlo a la acción correcta, sin necesidad de utilizar middleware, validaciones u otro tipo de estructura de control, tan solo haciendo uso del método __where()__ dentro el archivo de rutas.

El componente de rutas de Laravel permite capturar las peticiones realizadas por un usuario, de tal manera que podamos evaluar los parámetros enviados y devolver la respuesta acorde a cada petición. Cuando iniciamos un nuevo Proyecto, las rutas se establecen por defecto dentro del archivo routes.php en el directorio app\Http, en este archivo podrás ver algo similar a esto:

Route::get('/', function () {
    return view('welcome');
});
1
2
3
Route::get('/', function () {
    return view('welcome');
});
