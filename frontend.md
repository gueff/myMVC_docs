
# Frontend

- [Template Engine Smarty](#Template-Engine-Smarty)
- [View](#View)
- [Accessing Class methods and objects in smarty template](#Accessing-Class-methods-and-objects-in-smarty-template)

---

<a id="Template-Engine-Smarty"></a>
## Template Engine Smarty

myMVC makes use of the robust Template Engine `Smarty`. 

All Templates you define in your module's template folder.

myMVC provides a standard set of template files if you create your module via myMVC.phar (see: [Creating a Module](/3.x/creating-a-module)). Below you can see the directory structure of the standard set of templates.

_templates directory structure (assuming module is `Foo`)_  
~~~
modules/Foo/templates/
└── Frontend
    ├── content
    │   ├── 404.tpl
    │   ├── _cookieConsent.tpl
    │   ├── index.tpl
    │   └── _noscript.tpl
    └── layout
        ├── footer.tpl
        ├── header.tpl
        ├── index.tpl
        └── menu.tpl
~~~

_Smarty_  
For more Information about how to code templates with powerful Smarty Template Engine please visit the official Website https://www.smarty.net/ 

<a id="View"></a>
## View

**assign any variable to your template**

_assign any variable to your template (assuming module is `Foo`)_  
~~~php
\Foo\View\Index::init()->assign('myFrontendVariable', 'Any Content');
~~~

In your template you can access that assigned variable this way:

~~~html
{$myFrontendVariable}
~~~

**autoAssign variables** 

If you created your module via myMVC.phar (see: [Creating a Module](/3.x/creating-a-module)) or you added additional context information to your route by yourself, you can easily auto assign all [additional route infos](/3.x/routing#adding-additional-context-information-to-route): 

_autoAssign variables to template (assuming module is `Foo`)_  
~~~php
\Foo\View\Index::init()->autoAssign(
    Route::getCurrent()
);
~~~

**render**

_render the template (assuming module is `Foo`)_  
~~~php
\Foo\View\Index::init()->render();
~~~

<a id="Accessing-Class-methods-and-objects-in-smarty-template"></a>
## Accessing Class methods and objects in smarty template

You can access any myMVC Class directly in the templates.

_Examples_  
~~~php
// gives BasePath
{MVC\Config::get_MVC_BASE_PATH()}

// debugs additional info of the current route object
{MVC\Debug::info(MVC\Route::getCurrent()->get_additional())}
~~~

Also you can access methods of assigned Objects:

_Example_  
~~~php
{$oDTRoutingAdditional->get_sContent()}
~~~