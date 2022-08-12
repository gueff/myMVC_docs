
# CLI Wrapping

- [Requesting a route](#Requesting-a-route)
- [Requesting Controller directly; without a route](#Requesting-Controller-directly)

---

<a id="Requesting-a-route"></a>
**Requesting a route**

myMVC serves simple CLI Requests.

You can easily run any route via commandline;

just use the same path/query as in Frontend.

take care to place the request expression into single quotes !

_Examples_  
~~~
cd /public;
~~~
~~~bash
$ export MVC_ENV="develop"; php index.php '/'
$ export MVC_ENV="develop"; php index.php '/about/'
$ export MVC_ENV="develop"; php index.php '/about/?a={"foo":"bar"}'
~~~ 

---

<a id="Requesting-Controller-directly"></a>
**Requesting Controller directly; without a route**

myMVC allows calling via CLI without any need of a "/route/" (see below "Requesting routes").

Write Parameter separated by spaces.

When adding JSON in `a`-parameter, encapsulate with single quote `'`

_Example_  
~~~
cd /public;
~~~
~~~bash
$ export MVC_ENV="develop"; php index.php module=Foo c=Index m=index a='{"foo":"bar","baz":[1,2,3]}'
~~~