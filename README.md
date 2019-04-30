# Laravel
This repo has all the practice of my training in Laravel. It also serve as a documentation for the most commonly used commands.  Enjoy




## What we need?

you need to have PHP installed on your terminal, so you can test if you already has
you need to have php installed, then you can check if you have it installed with the command:

```php

php -v


alejandrodecastro@aledc:~$
PHP 7.1.23 (cli) (built: Nov 27 2018 16:59:25) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.1.0, Copyright (c) 1998-2018 Zend Technologies
```


Once we have verified that we have PHP, we proceed to install composer throught these comands:
Keep in mind that composer will be installed in the folder where you are.


```php

php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '48e3236262b34d30969dca3c37281b3b4bbe3221bda826ac6a9a62d6444cdb0dcd0615698a5cbe587c3f0fe57a54d8f5') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"

```
