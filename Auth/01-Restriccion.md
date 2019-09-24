![Laravel](https://raw.githubusercontent.com/aledc7/Laravel/master/pirullo.png "Aledc.com")

[![aledc.com](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/aledc.com.svg)](https://aledc.com)
[![ingenea.com.ar](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/ingenea.svg)](http://ingenea.com.ar)
[![License](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/mit-license.svg)](https://aledc.com)
[![GitHub release](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/release.svg)](https://aledc.com)
[![Dependencies](https://github.com/aledc7/Scrum-Certification/blob/master/recursos/dependencias-none.svg)](https://aledc.com)

##  Restricción de acceso con el Middleware Authenticate


## Rutas
En web.php encontramos diferentes rutas. __Auth::routes()__ incluye las rutas de inicio de sesión, registro y recuperación de contraseña.   
También tenemos la ruta de home, con el controlador __HomeController__.  

El controlador HomeController utiliza un middleware llamado auth:

```php
$this->middleware('auth');
````

Los middlewares son un mecanismo que trae el framework Laravel que permite filtrar las peticiones y modificar las respuestas antes de enviarlas al navegador.
es posible explorar los middlewares de nuestra aplicación en Kernel.php. En los middlewares asociados a rutas ($routeMiddleware) se encuentra el middleware auth que apunta a la clase Authenticate:
```php
protected $routeMiddleware = [
   'auth' => \Illuminate\Auth\Middleware\Authenticate::class, 
   //...
]
````

El middleware Authenticate se encuentra en el namespace __Illuminate\Auth\Middleware__.  
Si no estás utilizando un editor que te permita navegar los namespaces, puedes encontrarlo aquí: vendor/laravel/framework/src/Auth/Middleware/Authenticate.php.


En el middleware Authenticate existe un método llamado handle que es el método principal del middleware.  
Dentro de este método, Laravel llama a otro método con el nombre authenticate:
```php
public function handle($request, Closure $next, ...$guards)
{
    $this->authenticate($guards);

    return $next($request);
}
````

Nuevamente, en el método authenticate se va a llamar a otro método authenticate, pero esta vez del objeto auth de la clase AuthManager el cual a su vez va a llamar a un método del mismo nombre pero esta vez dentro de la clase SessionGuard.

```php
// Código en el middleware Authenticate:
protected function authenticate(array $guards)
{
    if (empty($guards)) {
        return $this->auth->authenticate();
    }

    /**/
}
````

## Excepciones
Laravel tiene un mecanismo de manejo de excepciones que podemos encontrar en Exceptions/Handler.php. En la clase Handler llamamos al método render. Este método render se encarga de revisar el tipo de excepción que se está arrojando. Por ejemplo, de ocurrir una excepción de autenticación se va a ejecutar el método unauthenticated:

```php
if ($e instanceof HttpResponseException) {
    return $e->getResponse();
} elseif ($e instanceof AuthenticationException) {
    return $this->unauthenticated($request, $e);
} elseif ($e instanceof ValidationException) {
    return $this->convertValidationExceptionResponse($e, $request);
}
````


Este método se va a encargar de devolver la respuesta adecuada dependiendo del tipo de petición.   
En el caso de que la petición espere JSON se va a enviar una respuesta de tipo JSON con el código HTTP 401 (no autorizado)   
Si se está esperando HTML entonces Laravel va a crear una redirección a la ruta de login:   
```php
return $request->expectsJson()
            ? response->json(['message' => $exception->getMessage()], 401)
            : redirect()->guest(route('login));
````

