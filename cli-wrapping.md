
# CLI Wrapping

- [Requesting a route](#Requesting-a-route)

---

<a id="Requesting-a-route"></a>
**Requesting a route**

myMVC serves simple CLI Requests.

You can easily run any existing route via commandline;

just use the same path/query as in Frontend.

take care to place the request expression into single quotes !

_Examples_  
~~~
cd /public;
~~~
~~~bash
$ php index.php '/'
$ php index.php '/about/'
$ php index.php '/about/?a={"foo":"bar"}'
~~~ 
