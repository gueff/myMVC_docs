<!--[:Getting Started]-->

# Directory Structure

## root

├── [application/](#application)  
├── config/  
├── [modules/](#modules-moduleName)  
├── public/  
├── composer.json  
├── myMVC.phar  
└── README.md  

---

<a id="application"></a>
### application/ 

The application directory contains core code of myMVC Framework and also working directories like `cache`, `log`, `session` and `templates_c`.

application/  
├── cache/  
├── config/  
├── doc/  
├── [library/](#application-library)  
├── log/  
├── session/  
├── smartyPlugins/  
├── templates_c/  
├── vendor/  
├── composer.json  
└── composer.phar  

- `cache/`: place for caching files
- `config/`: place for global config files
- `doc/`: GNU GENERAL PUBLIC LICENSE
- `log/`: logfile directory
- `session/`: SessionIDs are stored here
- `smartyPlugins/`: default smartyPlugin directory
- `templates_c/`: home for compiled smarty templates
- `vendor/`: third party libraries installed by composer
- `composer.json`: list of third party libraries to install
- `composer.phar`: a standalone composer script; no need to install composer


<a id="application-library"></a>
#### application/library/ 

This directory contains the core code of myMVC Framework inside the folder `MVC`.

~~~
application/library/
└── MVC/
    ├── DataType/
    │   ├── DTArrayObject.php
    │   ├── DTClass.php
    │   ├── DTConfig.php
    │   ├── DTConstant.php
    │   ├── DTKeyValue.php
    │   ├── DTProperty.php
    │   ├── DTRequestCurrent.php
    │   └── DTRoute.php
    ├── Generator/
    │   └── DataType.php
    ├── MVCInterface/
    │   ├── Controller.php
    ├── templates
    │   └── infoTool.tpl    
    ├── Application.php
    ├── Cache.php
    ├── Closure.php
    ├── Config.php
    ├── Controller.php
    ├── Convert.php
    ├── Debug.php
    ├── Error.php
    ├── Event.php
    ├── File.php
    ├── InfoTool.php
    ├── Lock.php
    ├── Log.php
    ├── Minify.php
    ├── phan.list
    ├── Policy.php
    ├── Reflex.php
    ├── Registry.php
    ├── Request.php
    ├── Route.php
    ├── Session.php
    ├── Strings.php    
    └── View.php
~~~

<a id="modules-moduleName"></a>
## modules/{moduleName}/ 

In the modules folder you create your own module (here in this example it is the module `Foo`).  
In your module you code your own application - You will find **Model**, **View**, **Controller** (MVC), but also some other directories.

modules/Foo/  
├── Controller/  
├── DataType/  
├── etc/  
├── Event/  
├── Model/  
├── Policy/  
├── [templates/](#modules-moduleName-templates)  
├── View/  
├── install.sh  
└── publish.sh  

- `Controller/`: folder to place your Controller
- `DataType/`: place for generated datatype classes
- `etc/`: place for install- and config files, docs, routing and individual other stuff
- `Event/`: place for classes dealing with events. This is optional;
- `Model/`: place for Model classes
- `Policy/`: classes for Policy
- `templates/`: smarty template files
- `View/`: View classes
- `install.sh`: helper bash script to install files from `modules/Foo/etc/_INSTALL/` to other places
- `publish.sh`: helper bash script to copy files from `modules/Foo/etc/_INSTALL/public/` to `public/`

_example: complete extracted directory of a module `Foo`_  
~~~
modules/Foo/
├── Controller/
│   └── Index.php
├── DataType/
│   └── DTRoutingAdditional.php
├── etc/
│   ├── config/
│   │   ├── DataType/
│   │   │   ├── datatype.php
│   │   │   └── _generate.php
│   │   ├── Foo/
│   │   │   ├── config
│   │   │   │   └── develop.php
│   │   │   └── composer.json
│   │   ├── _myMVC.php
│   │   └── policy.php
│   ├── doc/
│   ├── _INSTALL/
│   │   └── public/
│   │       └── robots.txt
│   └── routing/
│       └── frontend.php
├── Event/
│   └── Index.php
├── Model/
│   └── Index.php
├── Policy/
│   └── Index.php
├── templates/
│   └── Frontend/
│       ├── content/
│       │   ├── _cookieConsent.tpl
│       │   ├── _noscript.tpl
│       │   ├── 404.tpl
│       │   ├── index.tpl
│       │   └──  info.tpl
│       └── layout/
│           ├── footer.tpl
│           ├── index.tpl
│           └── menu.tpl
├── View/
│   └── Index.php
├── install.sh
└── publish.sh
~~~

<a id="modules-moduleName-templates"></a>
## modules/{moduleName}/templates 

_templates directory structure of a module `Foo`_  
~~~
modules/Foo/templates/
└── Frontend
    ├── content
    │   ├── 404.tpl
    │   ├── _cookieConsent.tpl
    │   ├── index.tpl
    │   ├── info.tpl
    │   └── _noscript.tpl
    └── layout
        ├── footer.tpl
        ├── index.tpl
        └── menu.tpl

~~~
- You may find further Information in Topic [Frontend](/3.x/frontend)