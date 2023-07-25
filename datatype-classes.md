
# DataType Classes

- [`DTRoute`](#DTRoute)
- [`DTRequestCurrent`](#DTRequestCurrent)
- [`DTFileinfo`](#DTFileinfo)

---

<a id="DTRoute"></a>
## `DTRoute`

- This Class is used to store Route Information.
- An object of this class gets being returned by `Route::getCurrent()` (see [/3.3.x/routing#Get-current-route](/3.3.x/routing#Get-current-route))

~~~php
/** @var \MVC\DataType\DTRoute $oDTRoute */
$oDTRoute = \MVC\Route::getCurrent();
~~~
~~~
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
    ["additional":protected]=>string(0) ""
}
~~~

<a id="DTRequestCurrent"></a>
## `DTRequestCurrent`

- This Class is used to store the current Request Information.
- An object of this class gets being returned by `Request::getCurrentRequest()` (see [/3.3.x/request#Get-current-Request](/3.3.x/request#Get-current-Request))

~~~php
/** @var \MVC\DataType\DTRequestCurrent $oDTRequestCurrent */
$oDTRequestCurrent = \MVC\Request::getCurrentRequest();
~~~
~~~
object(MVC\DataType\DTRequestCurrent)#17 (14) {
  ["scheme":protected]=>string(4) "http"
  ["host":protected]=>string(23) "mymvcdoku.ueffing.local"
  ["path":protected]=>string(23) "/3.3.x/datatype-classes"
  ["query":protected]=>string(0) ""
  ["queryArray":protected]=>array(0) {}
  ["requesturi":protected]=>string(23) "/3.3.x/datatype-classes"
  ["requestmethod":protected]=>string(3) "GET"
  ["protocol":protected]=>string(7) "http://"
  ["full":protected]=>string(53) "http://mymvcdoku.ueffing.local/3.3.x/datatype-classes"
  ["input":protected]=>string(0) ""
  ["isSecure":protected]=>bool(false)
  ["headerArray":protected]=>
  array(10) {
    ["Host"]=>string(23) "mymvcdoku.ueffing.local"
    ["Connection"]=>string(10) "keep-alive"
    ["Cache-Control"]=>string(9) "max-age=0"
    ["Upgrade-Insecure-Requests"]=>string(1) "1"
    ["User-Agent"]=>string(101) "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/106.0.0.0 Safari/537.36"
    ["Accept"]=>string(135) "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9"
    ["Referer"]=>string(64) "http://mymvcdoku.ueffing.local/3.3.x/generating-datatype-classes"
    ["Accept-Encoding"]=>string(13) "gzip, deflate"
    ["Accept-Language"]=>string(35) "de-DE,de;q=0.9,en-US;q=0.8,en;q=0.7"
    ["Cookie"]=>string(58) "myMVC_cookieConsent=true; myMVC=827rv9s19bfsk692j0pgje3kn2"
  }
  ["pathParam":protected]=>array(0) {}
  ["ip":protected]=>string(9) "127.0.0.1"
}
~~~

<a id="DTFileinfo"></a>
## `DTFileinfo`

- This Class is used to store File Information.
- An object of this class gets being returned by `File::info()`

~~~php
/** @var \MVC\DataType\DTFileinfo $oDTFileinfo */
$oDTFileinfo = \MVC\File::info(__FILE__);
~~~
~~~
object(MVC\DataType\DTFileinfo)#84 (16) {
  ["dirname":protected]=>string(151) "/var/www/myMVC/modules/Doc/Controller"
  ["basename":protected]=>string(9) "Index.php"
  ["path":protected]=>string(161) "/var/www/myMVC/modules/Doc/Controller/Index.php"
  ["is_file":protected]=>bool(true)
  ["is_dir":protected]=>bool(false)
  ["extension":protected]=>string(3) "php"
  ["filename":protected]=>string(5) "Index"
  ["name":protected]=>string(6) "admin1"
  ["passwd":protected]=>string(1) "x"
  ["uid":protected]=>int(1000)
  ["gid":protected]=>int(1000)
  ["gecos":protected]=>string(9) "admin1,,,"
  ["dir":protected]=>string(12) "/home/admin1"
  ["shell":protected]=>string(9) "/bin/bash"
  ["filemtime":protected]=>int(1666350030)
  ["filectime":protected]=>int(1666350030)
}
~~~

---

## `DTArrayObject`
## `DTKeyValue`
## `DTClass`
## `DTConfig`
## `DTConstant`
## `DTProperty`



