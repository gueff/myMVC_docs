
# Configuration

- [Environment `public/.env`](#Environment)
  - [MVC_ENV](#MVC_ENV)
  - [Custom .env variables](#custom-env-variables)
- [Config files, -places and reading order](#Config-files-places-and-reading-order)
  - [myMVC config folder](#myMVC-config-folder)
  - [Module's config folder](#Modules-config-folder)
  - [Module's environment config file](#Modules-environment-config-file)
- [Module's composer.json](#Modules-composer-json)
- [Access `MVC_*` config values](#Access-MVC-config-values)
- [Access module config values](#Access-module-config-values)

---

<a id="Environment"></a>
## Environment `public/.env`

<a id="MVC_ENV"></a>
**MVC_ENV**  
In the `public/` folder you will find an `.env` file. There the variable `MVC_ENV` is declared. This variable declares what Environment is valid for myMVC.  
You can name the value as you like - But common values are: `develop`, `test`, `production`.  

Make sure the corresponding environment config file does exist in your module  
(Schema: `/modules/{moduleName}/etc/config/{moduleName}/config/{environment}.php`).

_Example_  
If your Module is named `Foo`, and you set `MVC_ENV=production`,  
then the config file `/modules/Foo/etc/config/Foo/config/production.php` has to exist.

~~~bash
# Environment
MVC_ENV=production
~~~

<a id="custom-env-variables"></a>
**Custom .env variables**  
You are free to set up any `key=value` declarations you want in that `public/.env` file. 
Any `key=value` pair found in the file will be set by `putenv()` at start and you can access those variables via php's `getenv()` command.

_Example_  
~~~bash
# custom
foo=bar
~~~

_Check_  
~~~php
// string(3) "bar"
var_dump(
    getenv('foo')
);
~~~

---

<a id="Config-files-places-and-reading-order"></a>
## Config files, -places and reading order

The following locations for configurations exist and are read in succession in the appropriate order.

This also means that a later loaded configuration beats (overrides) an earlier loaded one.

1. ↓ myMVC config folder <a id="myMVC-config-folder"></a>
   - `/config/` 
   - Any `*.php` file in this directory will be included; Reading order is `a-z`
   - Coverage: **globally** - Configs placed here are valid to all modules and to all environments
   - By default, the following files are located here
      - `_myMVC.php`: This file contains basic settings that are necessary for operation. You should not edit this file. If you want to change settings, override values in your own module config. myMVC config variable names always begin with `MVC_`.  
2. ↓ Module's config folder <a id="Modules-config-folder"></a>
    - `/modules/{moduleName}/etc/config/`
    - Any `*.php` file in this directory will be included; Reading order is `a-z`
    - Coverage: **module globally** - Configs placed here are valid to all environments of your module
   - By default, the following files are located here
      - `_myMVC.php`: further MVC configs. myMVC config variable names always begin with `MVC_`.
3. ⤓ Module's environment config file <a id="Modules-environment-config-file"></a>
   - Schema: `/modules/{moduleName}/etc/config/{moduleName}/config/{environment}.php`
     - Example: If your module is named `Foo` and you have set `MVC_ENV` to `'develop'`, your environment config file has to be `modules/Foo/etc/config/Foo/config/develop.php`. Make sure it exists.
   - Coverage: **module environment specific** - The concrete environment config file is loaded appropiate to your environment of your module you have set in `MVC_ENV`


<a id="Modules-composer-json"></a>
## Module's composer.json

Extend your module with third libraries using composer. 

Write your requirements into the composer.json file which you find here:

_`composer.json`_  
~~~
/modules/{moduleName}/etc/config/{moduleName}/composer.json
~~~

The next time you run your myMVC Application, myMVC will automatically perform a `composer install`.  
Alternatively you can of course also perform an installation by hand.

_Installed vendor folder_  
~~~
/modules/{moduleName}/etc/config/{moduleName}/vendor
~~~

myMVC will automatically integrate all `/vendor/autoload.php` autoloaders from any myMVC module.


<a id="Access-MVC-config-values"></a>
## Access `MVC_*` config values 

Access MVC configurations via the `\MVC\Config` Class. For each variable there is an identical getter.

_Syntax_  
~~~
$mVar = \MVC\Config::get_{MVC_VARIABLE}();
~~~

_Example_  
~~~php
$sMvcBasePath = \MVC\Config::get_MVC_BASE_PATH();
~~~

Alternativley you can access all MVC Configs via `\MVC\Registry`.

_Alternative_  
~~~
$sMvcBasePath = \MVC\Registry::get('MVC_BASE_PATH');
~~~

<a id="Access-module-config-values"></a>
## Access module config values 

Access module configuration array via the `\MVC\Config` Class. 

_Example_
~~~php
$aModule = \MVC\Config::MODULE();
~~~

Alternativley you can access the module configuration array via `\MVC\Registry`.

_Alternative_
~~~
$aModule = \MVC\Registry::get('MODULE');
~~~