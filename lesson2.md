# Lesson 2

So, we have everything ready for start with our code.


```php
composer create-project laravel/laravel training-aledc "5.5.*"
```

In the command above, the last parameter "5.5. *" is optional, and is for indicate a specific version. I use this version because it is the same used in the documentation that I am using.

Now i can execute the php artisan serve command to run the project.

```php
php artisan serve
```


Let me show you how the structure of the generated folder like:


```php
/app
/bootstrap
/config
/database
/public
/resources
/routes
/storage
/tests
/vendor
composer.json
composer.lock
package.json
phpunit.xml
server.php
webpack.min.js
.env
.env.example
.gitignore
.gitatributes
```



All urls will be redirected to the index file. php located at:
```php
/public/index.php
```

All the files that are in the public folder will be precisely public, and all the files that are outside this folder will be private.

When you want to put the folder in production, you must point as the main path to:  /public/index.php
The App and user's routes requests are configured in the file:  /routes/web.php
Similarly, the routes of an API are defined in the file /routes/api.php


