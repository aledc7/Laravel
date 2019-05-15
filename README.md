![Laravel](https://github.com/aledc7/Laravel/blob/master/laravel_logo.png?raw=true "Aledc.com")


## Laravel 

## Level:  Intermediate
### Required Knowledge:  PHP Basic

This repo has all the practice of my training in Laravel. It also serve as a documentation for the most commonly used commands.  Enjoy


## Chapter 1 - Installing Composer and Laravel Installer

Composer is an application-level package manager for the PHP programming language that provides a standard format for managing dependencies of PHP software and required libraries.


### What we need for run these practice?

- [x] php 7 installed on your machine
- [x] console terminal
- [x] Internet, of course


you need to have php installed, then you can check if you have it installed with the command:

```php

php -v


alejandrodecastro@aledc:~$
PHP 7.1.23 (cli) (built: Nov 27 2018 16:59:25) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.1.0, Copyright (c) 1998-2018 Zend Technologies
```


Once we have verified that we have PHP, we proceed to install composer throught these four comands:
Keep in mind that composer will be installed in the folder where you are.


```php

php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

php -r "if (hash_file('sha384', 'composer-setup.php') === '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"

php composer-setup.php

php -r "unlink('composer-setup.php');"



For future updates always check the sourcecode:
https://getcomposer.org/download/

```

once installed we can verify if everything went well by executing the downloaded file composer.phar

```php
php composer.phar
```

This will show among other things the list of available commands.

```php
alejandrodecastro@aledc:~/test$ php composer.phar
   ______
  / ____/___  ____ ___  ____  ____  ________  _____
 / /   / __ \/ __ `__ \/ __ \/ __ \/ ___/ _ \/ ___/
/ /___/ /_/ / / / / / / /_/ / /_/ (__  )  __/ /
\____/\____/_/ /_/ /_/ .___/\____/____/\___/_/
                    /_/
Composer version 1.8.5 2019-04-09 17:46:47

Usage:
  command [options] [arguments]

Options:
  -h, --help                     Display this help message
  -q, --quiet                    Do not output any message
  -V, --version                  Display this application version
      --ansi                     Force ANSI output
      --no-ansi                  Disable ANSI output
  -n, --no-interaction           Do not ask any interactive question
      --profile                  Display timing and memory usage information
      --no-plugins               Whether to disable plugins.
  -d, --working-dir=WORKING-DIR  If specified, use the given directory as working directory.
  -v|vv|vvv, --verbose           Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Available commands:
  about                Shows the short information about Composer.
  archive              Creates an archive of this composer package.
  browse               Opens the package's repository URL or homepage in your browser.
  check-platform-reqs  Check that platform requirements are satisfied.
  clear-cache          Clears composer's internal package cache.
  clearcache           Clears composer's internal package cache.
  config               Sets config options.
  create-project       Creates new project from a package into given directory.
  depends              Shows which packages cause the given package to be installed.
  diagnose             Diagnoses the system to identify common errors.
  dump-autoload        Dumps the autoloader.
  dumpautoload         Dumps the autoloader.
  exec                 Executes a vendored binary/script.
  global               Allows running commands in the global composer dir ($COMPOSER_HOME).
  help                 Displays help for a command
  home                 Opens the package's repository URL or homepage in your browser.
  i                    Installs the project dependencies from the composer.lock file if present, or falls back on the composer.json.
  info                 Shows information about packages.
  init                 Creates a basic composer.json file in current directory.
  install              Installs the project dependencies from the composer.lock file if present, or falls back on the composer.json.
  licenses             Shows information about licenses of dependencies.
  list                 Lists commands
  outdated             Shows a list of installed packages that have updates available, including their latest version.
  prohibits            Shows which packages prevent the given package from being installed.
  remove               Removes a package from the require or require-dev.
  require              Adds required packages to your composer.json and installs them.
  run-script           Runs the scripts defined in composer.json.
  search               Searches for packages.
  self-update          Updates composer.phar to the latest version.
  selfupdate           Updates composer.phar to the latest version.
  show                 Shows information about packages.
  status               Shows a list of locally modified packages, for packages installed from source.
  suggests             Shows package suggestions.
  u                    Upgrades your dependencies to the latest version according to composer.json, and updates the composer.lock file.
  update               Upgrades your dependencies to the latest version according to composer.json, and updates the composer.lock file.
  upgrade              Upgrades your dependencies to the latest version according to composer.json, and updates the composer.lock file.
  validate             Validates a composer.json and composer.lock.
  why                  Shows which packages cause the given package to be installed.
  why-not              Shows which packages prevent the given package from being installed.
```

So far we have installed composer locally, which means that it will only work in the local folder where it was downloaded.

It can be a good idea to install it Globally, this way it will be possible to run it in any folder you want.

For this you should run the following comand in the folder where you installed it previously:

```php
mv composer.phar /usr/local/bin/composer
```

That's it, in this way we will have installed Composer on our computer.

For more info, check te official documentation [Composer](https://getcomposer.org/doc/00-intro.md)


Now, we can create our first project.

```php
composer create-project --prefer-dist laravel/laravel hellolaravel
```

ahother way is throught the Laravel Installer:

```php
composer global require "laravel/installer"
```

after this, you need to add the Path to your system.

in Mac:
```php
nano ~/.bash_profile
```

then, add this line, save and restart the terminal.

```php
export PATH=/usr/local/bin:$HOME/.composer/vendor/bin:$PATH
```


If everything went well, we will have Laravel installed and ready to run it in any folder.


You can finally try this by running

```php
laravel new  name-of-my-project
```

then to launch the server we use this command:

```php
php artisan serve
```

And now you can go to localhost:8000  and see your project running.



We complete the basic Laravel installation process  :)


We will continue with lesson 2








