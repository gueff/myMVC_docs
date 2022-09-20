
# Request

- [Get current Request](#Get-current-Request)
- [Get data from body of current Request](#Get-data-from-body-of-current-Request)
- [Accessing Path Params / Variables](#Accessing-Path-Params-Variables)
- [Sanitizing](#Sanitizing)

---

<a id="Get-current-Request"></a>
## Get current Request

_Example **GET** Request_
~~~
http://mymvc.ueffing.local/foo/bar/?a=1;b=2;c=3
~~~

_Command_  
~~~php
$oDTRequestCurrent = Request::getCurrentRequest()
~~~

_Example Result of `$oDTRequestCurrent`_
~~~
object(MVC\DataType\DTRequestCurrent)#65 (9) {
  ["scheme":protected]=>string(4) "http"
  ["host":protected]=>string(19) "mymvc.ueffing.local"
  ["path":protected]=>string(9) "/foo/bar/"
  ["query":protected]=>string(11) "a=1;b=2;c=3"
  ["requesturi":protected]=>string(21) "/foo/bar/?a=1;b=2;c=3"
  ["requestmethod":protected]=>string(3) "GET"
  ["protocol":protected]=>string(7) "http://"
  ["full":protected]=>string(47) "http://mymvc.ueffing.local/foo/bar/?a=1;b=2;c=3"
  ["input":protected]=>string(0) ""
}
~~~

As it gives you an object of type `MVC\DataType\DTRequestCurrent`, you can access all key/values by a getter.

_For example_  
~~~
$sPath = Request::getCurrentRequest()->get_path();
$sQuery = Request::getCurrentRequest()->get_query();
~~~

**check request method against route method**

Check if request method equals the expecting one you declared in your route.

~~~
Request::getServerRequestMethod() < = ? = > Route::getCurrent()->get_method()
~~~

_Example_  
~~~php 
if (Request::getServerRequestMethod() !== Route::getCurrent()->get_method())
{
    die('wrong request method `' 
        . Request::getServerRequestMethod() 
        . '`. It has to be: `' 
        . Route::getCurrent()->get_method() . '`'
    );
}
~~~

<a id="Get-data-from-body-of-current-Request"></a>
## Get data from body of current Request

_Example **PUT** Request_
~~~bash
curl -X PUT http://mymvc.ueffing.local/api/1.0.0/user/1969/ -H "Content-Type: application/json" -d '{"key": "value"}'
~~~

_Command_
~~~php
$sInput = Request::getCurrentRequest()->get_input();
~~~

_Example Result_
~~~
{"key": "value"}
~~~
- As you can see here, `input` contains the values we PUT (`{"key": "value"}`)


<a id="Accessing-Path-Params-Variables"></a>
## Accessing Path Params / Variables

_Example route_
~~~php
\MVC\Route::get('/api/:id/:name/:address/*', 'module=Foo&c=Api&m=index');
~~~
- _for more Information about setting up such routes, see [Routing with Path Params / Variables](/3.x/routing#path-params)_

_Example Request_
- `/api/1/Foo/Bar/what/else/`

<a id="Get-all-Variables"></a>
**Get all Variables**

_Command_
~~~php
$aPathParam = Request::getPathParam();
~~~

_Example Result of `$aPathParam`_
~~~
array(4) {
  ["id"]=>string(1) "1"
  ["name"]=>string(3) "Foo"
  ["address"]=>string(3) "Bar"
  ["_tail"]=>string(10) "what/else/"
}
~~~

<a id="Get-a-certain-Variable"></a>
**Get a certain Variable**

_Command_
~~~php
$sPathParam = Request::getPathParam('name')
~~~

_Example Result of `$sPathParam`_
~~~
Foo
~~~

<a id="Get-the-overlapping-string-on-wildcard-route-paths"></a>
**Get the overlapping string on wildcard route paths**

say you have a wildcard route and you want to get the overlapping path string after `*`.

_Example route_
~~~php
\MVC\Route::get('/foo/*', 'module=Foo&c=Index&m=foo');
~~~
- _see [Wildcard routing](/3.x/routing#wildcard-routing)_

_Example Request_
- `/foo/bar/baz/`

_Command_
~~~php
$sTail = Request::getPathParam()['_tail'];
~~~

_Result of `$sTail`_
~~~
bar/baz/
~~~

_If you want the Result as an array_
~~~php
$aTail = Request::getPathArray(
    Request::getPathParam()['_tail']
);
~~~

_Result of `$aTail`_
~~~
array(2) {
  [0]=>string(3) "bar"
  [1]=>string(1) "baz"
}
~~~

<a id="Sanitizing"></a>
## Sanitizing

You can define rules for sanitizing any `$_GET`, `$_POST`, `$_COOKIE` parameter for a request. 

*sanitizing `$_GET`, `$_POST`, `$_COOKIE`*  
~~~php 
Request::sanitize('GET', array(
    // rules for parameter `a`
    'a' => array(
        /** @see https://www.regular-expressions.info/unicode.html */
        'regex' => "/[^\\p{L}\\p{M}\\p{Z}\\p{S}\\p{N}\\p{P}\|']+/u",#
        'length' => 256,
    ),
));
~~~

_sanitizing input (e.g. `PUT`)_  
~~~php 
$oDTRequestCurrent = Request::getCurrentRequest();

// sanitizing
$oDTRequestCurrent->set_input(
    preg_replace(
        // sanitizing by regex rule
        "/[^\\p{L}\\p{M}\\p{Z}\\p{S}\\p{N}\\p{P}\|']+/u",
        '',
        // sanitizing by string length
        substr($oDTRequestCurrent->get_input(), 0, 256)
    )
);

// sanitized
$sInput = $oDTRequestCurrent->get_input();
~~~