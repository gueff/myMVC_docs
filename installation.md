
# Installation

- [Installing](#Installing_via_command_line) 
- [Create a new Module](#create_module)
- [Run myMVC](#Run_myMVC)
- [Requirements](#Requirements)
	- [Libraries via Composer Packages](#Libraries_via_Composer_Packages)
	- [Libs](#Libs)
	- [Autoloading](#Autoloading)

---

## Installing via command line <a name="Installing_via_command_line"></a>

e.g. for a `develop` Environment

~~~bash
$ export MVC_ENV="develop"; \
svn co https://github.com/gueff/myMVC.git/trunk/ myMVC; \
cd myMVC/public; php index.php
~~~

The Auto-Installer will instantly begin to install all necessary files. (In case of errors, a text will prompt up showing details about what went wrong). 

## Create a new Module <a name="create_module"></a>

run myMVC.phar, it will show you a menu with options to create a new Module on CLI.

~~~bash
$ php myMVC.phar
~~~

## Run myMVC <a name="Run_myMVC"></a>

run php's internal webserver:

~~~bash
cd /var/www/myMVC/public/; php -S localhost:1969;
~~~

Call `localhost:1969` in your browser. 

---

## Requirements <a name="Requirements"></a>

- PHP 7
    - shell_exec
    - mbstring; http://php.net/manual/en/mbstring.installation.php
    - safe_mode_allowed_env_vars; to enable to set MVC_ENV as an environment variable you may need to edit this php setting http://php.net/manual/en/function.putenv.php
- `MVC_ENV` set as environment variable e.g. `develop | test | live ` or other. This can be done in different ways:  
    -	webserver (recommended)
        - `SetEnv MVC_ENV develop` when using apache webserver (also see Section "Webserver")
        - `fastcgi_param  MVC_ENV production;` when using nginx webserver (also see Section "Webserver")
    -	config setting
        - `$aConfig['MVC_ENV'] = 'live'`; this config will even overwrite `MVC_ENV` var set by webserver. This is the way to go if you cannot however set webserver environments.
    -	`export MVC_ENV=live;` as CLI command

    once set, `MVC_ENV` can be accessed by
    -	`getenv('MVC_ENV')` and
    -	`\MVC\Registry::get('MVC_ENV')`
- write access for the webserver user (e.g. www-data) on folder:

    ~~~
    /application
    /application/session
    /application/cache
    /application/log
    /application/templates_c
    ~~~

### Libraries via Composer Packages <a name="Libraries_via_Composer_Packages"></a>

To install Libraries manually:  

~~~bash
cd application;
php composer.phar self-update; php composer.phar install;
~~~

Further Information about Composer / JSON Config File and Handling  

- https://packagist.org/
- https://getcomposer.org/doc/01-basic-usage.md
- https://www.digitalocean.com/community/articles/how-to-install-and-use-composer-on-your-vps-running-ubuntu

### Libs <a name="Libs"></a>

_Less_  

- http://leafo.net/lessphp/
- http://lessphp.gpeasy.com/#transitioning-from-leafolessphp

### Autoloading / PSR-0 <a name="Autoloading"></a>

As of 2014-10-21 PSR-0 has been marked as deprecated. @see http://www.php-fig.org/psr/psr-0/

