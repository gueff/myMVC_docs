<!--[Getting Started]-->
# Configuration

## Environment <a name="Environment"></a>

myMVC needs to know which config files to load; therefore you will need to tell myMVC what Environment currently should be set as active. And this is done my setting the ENV variable `MVC_ENV`.

### `public/.env`
in the `public/` folder you will find a `.env` file.  
There the MVC_ENV variable is declared.

~~~bash
# Environment
MVC_ENV=develop
~~~

## main config <a name="main_config"></a>

~~~
/config/
~~~

Any file in this directory which has suffix .php will be required automatically (Attention: Reading A-Z). This is the right place to extend or overwrite the main config **globally** (for all modules), to place global policies and so on.

_myMVC main config file_  
~~~
/config/_myMVC.php
~~~
- you should not edit this file
- if you want to change settings, override values in your own module config file


## module config file <a name="custom_config"></a>

_syntax_  
~~~bash
modules/{moduleName}/etc/config/{moduleName}/config/{stage}.php
~~~

_example: module=`Foo`, MVC\_ENV=`develop`_  
~~~bash
modules/Foo/etc/config/Foo/config/develop.php
~~~

if you have set `MVC_ENV` to `'develop'`, the folder `modules/Foo/etc/config/Foo/config/develop.php` would then be created automatically at runtime if it does not exist.
