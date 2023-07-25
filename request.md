
# Request

- [Get current Request](#Get-current-Request)
  - [Check request method against route method](#check-request-method-against-route-method)
- [Get data from header of current Request](#Get-data-from-header-of-current-Request)
  - [Get all headers](#Get-all-headers)
  - [Get a certain header](#Get-a-certain-header)
- [Get data from body of current Request](#Get-data-from-body-of-current-Request)
- [Accessing Path Params / Variables](#Accessing-Path-Params-Variables)
- [Sanitizing](#Sanitizing)

------------------------------------------------------------------------------------------------------------------------
<a id="Get-current-Request"></a>
## Get current Request

_Example **GET** Request_
~~~
http://mymvc.ueffing.local/foo/bar/?a=1;b=2;c=3
~~~

_Command_  
~~~php
$oDTRequestCurrent = \MVC\Request::getCurrentRequest()
~~~
- see [/3.3.x/datatype-classes#DTRequestCurrent](/3.3.x/datatype-classes#DTRequestCurrent)

As it gives you an object of type `MVC\DataType\DTRequestCurrent`, you can access all key/values by a getter.

_For example_  
~~~php
$sPath = \MVC\Request::getCurrentRequest()->get_path();
$sQuery = \MVC\Request::getCurrentRequest()->get_query();
~~~

<a id="check-request-method-against-route-method"></a>
**Check request method against route method**

Check if request method equals the expecting one you declared in your route.

_Check_    
~~~php 
$bMethodMatch = (
    // any request method is allowed
    '*' === \MVC\Route::getCurrent()->get_method() ||
    // request method does match route method
    \MVC\Request::getServerRequestMethod() === \MVC\Route::getCurrent()->get_method()
) ? true : false;
~~~

_Evaluate and React_  
~~~php
if (false === $bMethodMatch)
{
    die('wrong request method `' 
        . \MVC\Request::getServerRequestMethod() 
        . '`. It has to be one of: `' 
        . implode('|', Route::getCurrent()->get_methodsAssigned()) . '`'
    );
}
~~~

------------------------------------------------------------------------------------------------------------------------
<a id="Get-data-from-header-of-current-Request"></a>
## Get data from header of current Request

<a id="Get-all-headers"></a>
**Get all headers** 

_Command_
~~~php
$aHeader = \MVC\Request::getHeaderArray();
~~~

_Example Result_
~~~
array(10) {
  ["Host"]=>string(23) "mymvcdoku.ueffing.local"
  ["Connection"]=>string(10) "keep-alive"
  ["Cache-Control"]=>string(9) "max-age=0"
  ["Upgrade-Insecure-Requests"]=>string(1) "1"
  ["User-Agent"]=>string(101) "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/106.0.0.0 Safari/537.36"
  ["Accept"]=>string(135) "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9"
  ["Referer"]=>string(44) "http://mymvcdoku.ueffing.local/3.3.x/request"
  ["Accept-Encoding"]=>string(13) "gzip, deflate"
  ["Accept-Language"]=>string(35) "de-DE,de;q=0.9,en-US;q=0.8,en;q=0.7"
  ["Cookie"]=>string(58) "myMVC_cookieConsent=true; myMVC=0j6eatdmvbq8tqsnsoeph6kipd"
}
~~~

<a id="Get-a-certain-header"></a>
**Get a certain header**

_Command_
~~~php
$aHeader = \MVC\Request::getHeader('Connection');
~~~

_Example Result_
~~~
string(10) "keep-alive"
~~~

------------------------------------------------------------------------------------------------------------------------
<a id="Get-data-from-body-of-current-Request"></a>
## Get data from body of current Request

_Example **PUT** Request_
~~~bash
curl -X PUT http://mymvc.ueffing.local/api/1.0.0/user/1969/ -H "Content-Type: application/json" -d '{"key": "value"}'
~~~

_Command_
~~~php
$sInput = \MVC\Request::getCurrentRequest()->get_input();
~~~

_Example Result_
~~~
{"key": "value"}
~~~
- As you can see here, `input` contains the values we PUT (`{"key": "value"}`)

------------------------------------------------------------------------------------------------------------------------
<a id="Accessing-Path-Params-Variables"></a>
## Accessing Path Params / Variables

_Example route_
~~~php
\MVC\Route::get('/api/:id/:name/:address/*', 'module=Foo&c=Api&m=index');
~~~
- _for more Information about setting up such routes, see [Routing with Path Params / Variables](/3.3.x/routing#path-params)_

_Example Request_
- `/api/1/Foo/Bar/what/else/`

<a id="Get-all-Variables"></a>
**Get all Variables**

_Command_
~~~php
$aPathParam = \MVC\Request::getPathParam();
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
$sPathParam = \MVC\Request::getPathParam('name')
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
- _see [Wildcard routing](/3.3.x/routing#wildcard-routing)_

_Example Request_
- `/foo/bar/baz/`

_Command_
~~~php
$sTail = \MVC\Request::getPathParam()['_tail'];
~~~

_Result of `$sTail`_
~~~
bar/baz/
~~~

_If you want the Result as an array_
~~~php
$aTail = \MVC\Request::getPathArray(
    \MVC\Request::getPathParam()['_tail']
);
~~~

_Result of `$aTail`_
~~~
array(2) {
  [0]=>string(3) "bar"
  [1]=>string(1) "baz"
}
~~~

------------------------------------------------------------------------------------------------------------------------
<a id="Sanitizing"></a>
## Sanitizing

You can define rules for sanitizing any `$_GET`, `$_POST`, `$_COOKIE` parameter for a request. 

*sanitizing `$_GET`, `$_POST`, `$_COOKIE`*  
~~~php 
\MVC\Request::sanitize('GET', array(
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
$oDTRequestCurrent = \MVC\Request::getCurrentRequest();

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