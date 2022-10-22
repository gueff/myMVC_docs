
# Routing

- [Creating a Route](#Creating-a-Route)
    - [Routing files](#Routing-files)
    - [Writing a Route](#writing-a-Route)
      - [Standard RESTful Request Methods](#Standard-RESTful-Request-Methods)
      - [Any Request Method](#Any-Request-Method")
      - [Mixed Request Methods](#Mixed-Request-Methods)
    - [Naming the target controller](#Naming-the-target-controller)
    - [Adding additional context information to route](#adding-additional-context-information-to-route)
    - [Wildcard routing](#wildcard-routing)
    - [Routing with Path Params / Variables](#path-params)
- [Working with Routes](#Working-with-Routes)
    - [Get all routes](#Get-all-routes)
        - [Get routes array](#get-routes-array)
        - [Get array stacked by request methods](#Get-array-stacked-by-request-methods)
    - [Get a route](#Get-a-route)
        - [Get current route](#Get-current-route)
        - [Get any route](#Get-any-route)
    - [Accessing additional context information](#Accessing-additional-context-information)
        - [Get the additional context information of the current route](#Get-the-additional-context-information-of-the-current-route)
        - [Get the additional context information of _any_ route](#Get-the-additional-context-information-of-any-route)
        - [Converting JSON back to object](#Converting-JSON-back-to-object)

---

<a id="Creating-a-Route"></a>
## Creating a Route

<a id="Routing-files"></a>
### Routing files

All routing information are written in php files in your module's routing folder. All `*.php` files in the routing folder will be load at start.

_Schema_
~~~
modules/{Module}/etc/routing/
~~~

If your module is `Foo`, then this is your routing folder
~~~
modules/Foo/etc/routing/;
~~~

per default you see these contents
~~~
modules/Foo/etc/routing/
└── frontend.php
~~~

_individual routing files_  
you can create you own `*.php` routing file, for example `foo.php` and edit your route in that file.

If you want to create routes for an API, it then makes sense to create a file called `api.php` and write all your api routes inside that file.


<a id="writing-a-Route"></a>
### Writing a Route

_TLDR; Examples_  
~~~php
\MVC\Route::GET     ('/', 'module=Foo&c=Index&m=index'); // expecting GET
\MVC\Route::POST    ('/', 'module=Foo&c=Index&m=index'); // expecting POST
\MVC\Route::PUT     ('/', 'module=Foo&c=Index&m=index'); // expecting PUT
\MVC\Route::DELETE  ('/', 'module=Foo&c=Index&m=index'); // expecting DELETE

\MVC\Route::ANY     ('/', 'module=Foo&c=Index&m=index');  // be open for any request method
\MVC\Route::MIX     (['GET', 'POST'], '/',    'module=Foo&c=Index&m=index'); // expecting GET or POST
~~~

<a id="Standard-RESTful-Request-Methods"></a>
**Standard RESTful Request Methods**

_You declare a route with Command `\MVC\Route`:_
~~~
\MVC\Route::{METHOD}(string path, string targetController, mixed additionalInformation)
~~~
- `{METHOD}`: one of the RESTful request methods `GET`, `POST`, `PUT`, `DELETE`; you declare which RESTful request method is expected for this route
- `path`: url path
- `targetController`: the `Module`, `Controller` and `method` the route leads to
- `additionalInformation` [optional]: any information you may need to process in your target controller (or elsewhere). You can pass string, array, object, bool or whatever you like.

_Example_  
~~~php
\MVC\Route::GET('/foo/', 'module=Foo&c=Index&m=index');
~~~

Assuming you are running myMVC's local development server, you can then call the url `http://127.0.0.1:1969/foo/`

<a id="Any-Request-Method"></a>
**Any Request Method**

_Using `ANY` you leave it open which request method should apply_   
~~~php
\MVC\Route::ANY('/foo/', 'module=Foo&c=Index&m=index');
~~~

<a id="Mixed-Request-Methods"></a>
**Mixed Request Methods**

_Assign more than one request method to the route with `MIX`_
~~~php
\MVC\Route::MIX(['GET', 'POST'], '/foo/', 'module=Foo&c=Index&m=index');
~~~


<a id="Naming-the-target-controller"></a>
### Naming the target controller

You can name the target controller in two ways.

**query notation**

historically conditioned you can still name it with the myMVC's query notation

~~~php
\MVC\Route::GET('/foo/', 'module=Foo&c=Index&m=index');
~~~
- request method here is `GET`
- path is `/foo/`
- targetController: leads to => Module `Foo` (module), Controller `Index` (c), Method `index` (m) => (which is Class::method `\Foo\Controller\Index::index`)

**controller::method notation**

You can also name the target controller and method info by its class and method names itself.  
⚠ Requirement: It has to be a MVC Controller.

~~~php
\MVC\Route::GET('/foo/', '\Foo\Controller\Index::index');
~~~
- request method here is `GET`
- path is `/foo/`
- targetController: leads to Class::method `\Foo\Controller\Index::index`

<a id="adding-additional-context-information-to-route"></a>
### Adding additional context information to route

You can add any information to the route as long as it is a string or any other type that could be encoded to JSON - type `array` for example.

The information you add to your route will be `json_encode()` and therefore stored as JSON. When you read the additional information you get JSON.

**Simple Examples**

~~~php
\MVC\Route::GET('/foo/', '\Foo\Controller\Index::index', 'Hello World');
~~~
~~~php
\MVC\Route::GET('/foo/', '\Foo\Controller\Index::index', [1, 'foo', 'bar']);
~~~

**More complex Example**

In the example `frontend.php` you see a DataType Class `$oDTRoutingAdditional` of Type `\Foo\DataType\DTRoutingAdditional`.
This class is generated by myMVC's DataType Generator; see [/3.2.x/generating-datatype-classes](/3.2.x/generating-datatype-classes) for more Information.

The information stored in this DataType Class will be accessed by the View Class as they are necessary for setting up the View correctly.

As we noted earlier, the additional information you want to add to your route have to be string or any other type that could be encoded to JSON.
Luckily the DataType class per default has a method to serve its contents as JSON:
~~~php 
$oDTRoutingAdditional->getPropertyJson()
~~~

_Example object DTRoutingAdditional_
~~~php
$oDTRoutingAdditional = \Foo\DataType\DTRoutingAdditional::create()
    ->set_sTitle('Foo')
    ->set_sLayout('Frontend/layout/index.tpl')
    ->set_sMainmenu('Frontend/layout/menu.tpl')
    ->set_sContent('Frontend/content/index.tpl')
    ->set_sHeader('Frontend/layout/header.tpl')
    ->set_sNoscript('Frontend/content/_noscript.tpl')
    ->set_sCookieConsent('Frontend/content/_cookieConsent.tpl')
    ->set_sFooter('Frontend/layout/footer.tpl')
    ->set_aStyle(array (
        '/myMVC/assets/bootstrap-4.4.1-dist/css/bootstrap.min.css',
        '/myMVC/assets/font-awesome-4.7.0/css/font-awesome.min.css',
        '/myMVC/styles/myMVC.min.css',
    ))
    ->set_aScript(array (
        '/myMVC/assets/jquery/3.4.1/jquery-3.4.1.min.js',
        '/myMVC/assets/jquery-cookie/1.4.1/jquery.cookie.min.js',
        '/myMVC/assets/bootstrap-4.4.1-dist/js/bootstrap.min.js',
        '/myMVC/scripts/cookieConsent.min.js',
    ));
~~~

_Example route_
~~~php
\MVC\Route::GET(
    '/foo/', 
    '\Foo\Controller\Index::index', 
    $oDTRoutingAdditional->getPropertyJson()
);
~~~

<a id="wildcard-routing"></a>
### Wildcard routing

**per default all routings are restricitv**  

_Example_  
~~~php
\MVC\Route::GET('/foo/', 'module=Foo&c=Index&m=foo');
~~~

you can only request  
- `/foo/`

you cannot request e.g.  
- `/foo/bar/`

**activating wildcard routing**  
therefore, just add `*` after the path: `'/foo/*'`.  

~~~php
\MVC\Route::GET('/foo/*', 'module=Foo&c=Index&m=foo');
~~~

now you can call the route with further paths 
- e.g. `/foo/bar/baz/`

You can get the tailing path after `/foo/` by accessing Path Params _tail.

~~~php
$aPathParam = \MVC\Request::getPathParam()['_tail'];
~~~

this will give you

~~~
bar/baz/
~~~

- see `Request`: [Accessing Path Params / Variables](/3.2.x/request#Accessing-Path-Params-Variables)


<a id="path-params"></a>
### Routing with Path Params / Variables

You can define Routes with Variables.  
This you can do in one of two ways:

1. `:var` notation: e.g. `'/api/:id/:name/:address/'`  
2. `{var}` notation: e.g. `'/api/{id}/{name}/{address}/'`

You cannot mix these notations; Use one or the other for defining your routes.  
All variables you declare in your route are accessible by name.

_Examples_
~~~php
# :var notation
\MVC\Route::GET('/api/:id/:name/:address/', 'module=Foo&c=Api&m=index');
# {var} notation
\MVC\Route::GET('/api/{id}/{name}/{address}/', 'module=Foo&c=Api&m=index');
~~~

Valid Requests:
- `/api/1/Foo/Bar/`
- `/api/1969/FooBar/Somwhere%20else/`

_Example with activating wildcard routing_
~~~php
\MVC\Route::GET('/api/:id/:name/:address/*', 'module=Foo&c=Api&m=index');
~~~

Valid Requests:
- `/api/1/Foo/Bar/what/else/`
- `/api/1969/FooBar/Somwhere%20else/what/else/`

_Access the Variables_  
- see `Request`: [Accessing Path Params / Variables](/3.2.x/request#Accessing-Path-Params-Variables)

---

<a id="Working-with-Routes"></a>
## Working with Routes

<a id="Get-all-routes"></a>
### Get all routes

<a id="get-routes-array"></a>
**Get routes array**

Simply access the public property `$aRoute` of class `Route`. It will give you an associative array where its values are objects of type `MVC\DataType\DTRoute`.

_Command_
~~~php
/** @var MVC\DataType\DTRoute[] $aRoute */
$aRoute = Route::$aRoute;
~~~

_Example Result of `$aRoute` (shortened)_
~~~
array(2) {
  ["/"]=>
  object(MVC\DataType\DTRoute)#15 (9) {
    ["path":protected]=>string(1) "/"
    ["method":protected]=>string(3) "GET"
    ["methodsAssigned":protected]=>array(1) {[0]=> string(3) "GET"}
    ["query":protected]=>string(30) "module=Foo&c=Index&m=index"
    ["class":protected]=>string(24) "Foo\Controller\Index"
    ["classFile":protected]=>string(128) "/var/www/myMVC/modules/Foo/Controller/Index.php"
    ["module":protected]=>string(7) "Foo"
    ["c":protected]=>string(5) "Index"
    ["m":protected]=>string(5) "index"
    ["additional":protected]=>string(733) "{"sTitle":"Foo","sLayout":"Frontend\/layout\/index.tpl","sMainmenu":"Frontend\/layout\/menu.tpl","sContent":"Frontend\/content\/index.tpl","sHeader":"Frontend\/layout\/header.tpl","sNoscript":"Frontend\/content\/_noscript.tpl","sCookieConsent":"Frontend\/content\/_cookieConsent.tpl","sFooter":"Frontend\/layout\/footer.tpl","aStyle":["\/myMVC\/assets\/bootstrap-4.4.1-dist\/css\/bootstrap.min.css","\/myMVC\/assets\/font-awesome-4.7.0\/css\/font-awesome.min.css","\/myMVC\/styles\/myMVC.min.css"],"aScript":["\/myMVC\/assets\/jquery\/3.4.1\/jquery-3.4.1.min.js","\/myMVC\/assets\/jquery-cookie\/1.4.1\/jquery.cookie.min.js","\/myMVC\/assets\/bootstrap-4.4.1-dist\/js\/bootstrap.min.js","\/myMVC\/scripts\/cookieConsent.min.js"]}"
  }
  ["/404/"]=>
  object(MVC\DataType\DTRoute)#17 (9) {
   ...
 }
}
~~~

<a id="get-array-stacked-by-request-methods"></a>
**Get array stacked by request methods**

Access the public property `$aMethod` to get an array of route indeces stacked by request method.

_Command_
~~~php
$aMethod = Route::$aMethod;
~~~

_Example Result of `$aMethod`_
~~~
array(2) {
  ["put"]=>array(1) {
    [0]=>string(20) "/api/1.0.0/user/:id/"
  }
  ["get"]=>array(3) {
    [0]=>string(1) "/"
    [1]=>string(5) "/404/"
    [2]=>string(25) "/api/:id/:name/:address/*"
  }
}
~~~

<a id="Get-a-route"></a>
### Get a route

<a id="Get-current-route"></a>
#### Get current route

this will give you the current route object to the requested url path.

_Example request_
- `/api/1/Foo/Bar/what/else/`

_Command_
~~~php
$oDTRoute = \MVC\Route::getCurrent();
~~~

_Example Result of `$oDTRoute`_
~~~
object(MVC\DataType\DTRoute)#19 (9) {
  ["path":protected]=>string(25) "/api/:id/:name/:address/*"
  ["method":protected]=>string(3) "GET"
  ["methodsAssigned":protected]=>array(1) {[0]=>string(3) "GET"}
  ["query":protected]=>string(30) "module=Foo&c=Api&m=index"
  ["class":protected]=>string(24) "Foo\Controller\Api"
  ["classFile":protected]=>string(128) "/var/www/myMVC/modules/Foo/Controller/Api.php"
  ["module":protected]=>string(7) "Foo"
  ["c":protected]=>string(5) "Index"
  ["m":protected]=>string(5) "index"
  ["additional":protected]=>string(727) "{"sTitle":"API","sLayout":"Frontend\/layout\/index.tpl","sMainmenu":"Frontend\/layout\/menu.tpl","sContent":"Frontend\/content\/foo.tpl","sHeader":"Frontend\/layout\/header.tpl","sNoscript":"Frontend\/content\/_noscript.tpl","sCookieConsent":"Frontend\/content\/_cookieConsent.tpl","sFooter":"Frontend\/layout\/footer.tpl","aStyle":["\/myMVC\/assets\/bootstrap-4.4.1-dist\/css\/bootstrap.min.css","\/myMVC\/assets\/font-awesome-4.7.0\/css\/font-awesome.min.css","\/myMVC\/styles\/myMVC.min.css"],"aScript":["\/myMVC\/assets\/jquery\/3.4.1\/jquery-3.4.1.min.js","\/myMVC\/assets\/jquery-cookie\/1.4.1\/jquery.cookie.min.js","\/myMVC\/assets\/bootstrap-4.4.1-dist\/js\/bootstrap.min.js","\/myMVC\/scripts\/cookieConsent.min.js"]}"
}
~~~

As the Result is an object of type `MVC\DataType\DTRoute` you can access all its properties by getter.

_Example_ 
~~~php
$sClass = \MVC\Route::getCurrent()->get_methodsAssigned();
~~~

_Example Result of `$sClass`_  
~~~
array(1) {
  [0]=>
  string(3) "GET"
}
~~~

<a id="Get-any-route"></a>
#### Get any route

you can get any route object by accessing public `$aRoute` from class `Route`.

_Command_
~~~php
// we want the route object of route /404/
$oRoute = \MVC\Route::$aRoute['/404/'];
~~~

_Example Result of `$oRoute`_  
~~~
object(MVC\DataType\DTRoute)#17 (9) {
  ["path":protected]=>string(5) "/404/"
  ["method":protected]=>string(3) "GET"
  ["methodsAssigned":protected]=>array(1) {[0]=>string(3) "GET"}  
  ["query":protected]=>string(33) "module=Foo&c=Index&m=notFound"
  ["class":protected]=>string(24) "Foo\Controller\Index"
  ["classFile":protected]=>string(128) "/var/www/myMVC/modules/Foo/Controller/Index.php"
  ["module":protected]=>string(7) "Foo"
  ["c":protected]=>string(5) "Index"
  ["m":protected]=>string(8) "notFound"
  ["additional":protected]=>string(727) "{"sTitle":"404","sLayout":"Frontend\/layout\/index.tpl","sMainmenu":"Frontend\/layout\/menu.tpl","sContent":"Frontend\/content\/404.tpl","sHeader":"Frontend\/layout\/header.tpl","sNoscript":"Frontend\/content\/_noscript.tpl","sCookieConsent":"Frontend\/content\/_cookieConsent.tpl","sFooter":"Frontend\/layout\/footer.tpl","aStyle":["\/myMVC\/assets\/bootstrap-4.4.1-dist\/css\/bootstrap.min.css","\/myMVC\/assets\/font-awesome-4.7.0\/css\/font-awesome.min.css","\/myMVC\/styles\/myMVC.min.css"],"aScript":["\/myMVC\/assets\/jquery\/3.4.1\/jquery-3.4.1.min.js","\/myMVC\/assets\/jquery-cookie\/1.4.1\/jquery.cookie.min.js","\/myMVC\/assets\/bootstrap-4.4.1-dist\/js\/bootstrap.min.js","\/myMVC\/scripts\/cookieConsent.min.js"]}"
}
~~~

As the Result is an object of type `MVC\DataType\DTRoute` you can access all its properties by getter.

_Example_  
~~~php
$sClass = \MVC\Route::$aRoute['/404/']->get_class();
~~~

_Example Result of `$sClass`_  
~~~
Foo\Controller\Index
~~~


<a id="Accessing-additional-context-information"></a>
### Accessing additional context information

<a id="Get-the-additional-context-information-of-the-current-route"></a>
**Get the additional context information of the current route**

_Command_
~~~php
$sAdditional = \MVC\Route::getCurrent()->get_additional()
~~~

_Example Result of `$sAdditional` (JSON)_  
~~~json
{"sTitle":"Foo","sLayout":"Frontend\/layout\/index.tpl","sMainmenu":"Frontend\/layout\/menu.tpl","sContent":"Frontend\/content\/index.tpl","sHeader":"Frontend\/layout\/header.tpl","sNoscript":"Frontend\/content\/_noscript.tpl","sCookieConsent":"Frontend\/content\/_cookieConsent.tpl","sFooter":"Frontend\/layout\/footer.tpl","aStyle":["\/myMVC\/assets\/bootstrap-4.4.1-dist\/css\/bootstrap.min.css","\/myMVC\/assets\/font-awesome-4.7.0\/css\/font-awesome.min.css","\/myMVC\/styles\/myMVC.min.css"],"aScript":["\/myMVC\/assets\/jquery\/3.4.1\/jquery-3.4.1.min.js","\/myMVC\/assets\/jquery-cookie\/1.4.1\/jquery.cookie.min.js","\/myMVC\/assets\/bootstrap-4.4.1-dist\/js\/bootstrap.min.js","\/myMVC\/scripts\/cookieConsent.min.js"]}
~~~

<a id="Get-the-additional-context-information-of-any-route"></a>
**Get the additional context information of _any_ route**

Therefore access the public property `$aRoute` directly with the path of the route of your interest. It will give you an object of Type `MVC\DataType\DTRoute`, which allows you to access its method `get_additional()`.

_Command_
~~~php
$sAdditional = \MVC\Route::$aRoute['/404/']->get_additional()
~~~

_Example Result of `$sAdditional` (JSON)_
~~~json
{"sTitle":"404","sLayout":"Frontend\/layout\/index.tpl","sMainmenu":"Frontend\/layout\/menu.tpl","sContent":"Frontend\/content\/404.tpl","sHeader":"Frontend\/layout\/header.tpl","sNoscript":"Frontend\/content\/_noscript.tpl","sCookieConsent":"Frontend\/content\/_cookieConsent.tpl","sFooter":"Frontend\/layout\/footer.tpl","aStyle":["\/myMVC\/assets\/bootstrap-4.4.1-dist\/css\/bootstrap.min.css","\/myMVC\/assets\/font-awesome-4.7.0\/css\/font-awesome.min.css","\/myMVC\/styles\/myMVC.min.css"],"aScript":["\/myMVC\/assets\/jquery\/3.4.1\/jquery-3.4.1.min.js","\/myMVC\/assets\/jquery-cookie\/1.4.1\/jquery.cookie.min.js","\/myMVC\/assets\/bootstrap-4.4.1-dist\/js\/bootstrap.min.js","\/myMVC\/scripts\/cookieConsent.min.js"]}
~~~

<a id="Converting-JSON-back-to-object"></a>
**Converting JSON back to object**

You first have to convert that JSON Result into array.  
Then put that array into its DataType create method.

_Command_
~~~php
$oDTRoutingAdditional = \Foo\DataType\DTRoutingAdditional::create(
    json_decode(\MVC\Route::$aRoute['/404/']->get_additional(), true)
)
~~~

_Example Result of `$oDTRoutingAdditional`_  
~~~
object(Foo\DataType\DTRoutingAdditional)#66 (10) {
  ["sTitle":protected]=>string(3) "404"
  ["sLayout":protected]=>string(25) "Frontend/layout/index.tpl"
  ["sMainmenu":protected]=>string(24) "Frontend/layout/menu.tpl"
  ["sContent":protected]=>string(24) "Frontend/content/404.tpl"
  ["sHeader":protected]=>string(26) "Frontend/layout/header.tpl"
  ["sNoscript":protected]=>string(30) "Frontend/content/_noscript.tpl"
  ["sCookieConsent":protected]=>string(35) "Frontend/content/_cookieConsent.tpl"
  ["sFooter":protected]=>string(26) "Frontend/layout/footer.tpl"
  ["aStyle":protected]=>
  array(3) {
    [0]=>string(56) "/myMVC/assets/bootstrap-4.4.1-dist/css/bootstrap.min.css"
    [1]=>string(57) "/myMVC/assets/font-awesome-4.7.0/css/font-awesome.min.css"
    [2]=>string(27) "/myMVC/styles/myMVC.min.css"
  }
  ["aScript":protected]=>
  array(4) {
    [0]=>string(46) "/myMVC/assets/jquery/3.4.1/jquery-3.4.1.min.js"
    [1]=>string(54) "/myMVC/assets/jquery-cookie/1.4.1/jquery.cookie.min.js"
    [2]=>string(54) "/myMVC/assets/bootstrap-4.4.1-dist/js/bootstrap.min.js"
    [3]=>string(35) "/myMVC/scripts/cookieConsent.min.js"
  }
}
~~~