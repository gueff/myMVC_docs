
# Routing

- [Creating a Route](#Creating-a-Route)
    - [Wildcard routing](#wildcard-routing)
    - [Routing with Path Params](#path-params)

---

<a id="Creating-a-Route"></a>
## Creating a Route 

_Place of routing files_  
~~~
modules/{Module}/etc/routing/
~~~

so if your moudule is `Foo`, open your routing folder like this
~~~bash
modules/Foo/etc/routing/;
~~~

open the file `frontend.php` - beneath the existing rule, add your own.

- To keep it simple at start, just copy the existing rule of `"/"` and add it beneath as route `"/foo/"` - see route example.
- then you can call the url `http://{whatever}/foo/`

_individual routing files_  
you can create you own `*.php` routing file, for example `foo.php` and edit your route in that file.

If you want to create routes for an API, it then makes sense to create a file called `api.php` and write all your api routes inside that file.

_route examples: adding route `/foo/`_  
~~~php
\MVC\Route::get('/foo/', 'module=Foo&c=Index&m=index');
~~~
- Route `/foo/`
- leads to => Module `Foo`, Controller `Index`, Method `index`

<a id="wildcard-routing"></a>
### Wildcard routing   

**per default all routings are restricitv**  
e.g. the route `/foo/`  
- you can only call `/foo/`
- you can not call `/foo/bar/`

**activating wildcard routing**  
therefore, just add `*` after the route.  
e.g. the route `/foo/*`  
- you can call `/foo/`
- you can call `/foo/bar/`
- you can call `/foo/bar/baz/...`

<a id="path-params"></a>
### Routing with Path Params

You can define Routes with Variables, which you simply write with a leading `:`

_Example_  
~~~php
\MVC\Route::get('/api/:id/:name/:address/*', 'module=Webbixx&c=Api&m=foo');
~~~

Valid Requests are:
- `/api/1/Guido/Wallenhorst/`
- `/api/1969/FooBar/Somwhere%20else/`

and as long as we add an asterisk `*` at the end, the declared route becomes wildcard.    
So, these are also valid Requests:  
- `/api/1/Guido/Wallenhorst/foo/bar/`
- `/api/1969/FooBar/Somwhere%20else/foo/bar/john/doe/`

**Requesting the Variables** 

_get all  Variables_  
~~~php
Request::getPathParam(); # array
~~~

_get a certain Variable_  
~~~php
Request::getPathParam( $sKey ) # string
~~~

