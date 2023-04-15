
# Events

- [`\MVC\Event::run()`](#event-run)
- [`\MVC\Event::bind()`](#event-bind)
  - [`\MVC\Event::processBindConfigStack()` - Bind to Events via config Stack](#processBindConfigStack)
- [`\MVC\Event::delete()`](#event-delete) 
- [Early Event bondings](#Early-Event-bondings)
- [myMVC Standard Events](#myMVCStandardEvents)

------------------------------------------------------------------------------------------------------------------------

<a id="event-run"></a>
## `\MVC\Event::run()` 

with `run` you initialize an event; bonded Closures to the event name get executed in order.

_Example: Run a simple Event_
~~~php
\MVC\Event::run('foo');
~~~

_Example: pass an `DTArrayObject` Object with Infos_  
~~~php
\MVC\Event::run('foo', DTArrayObject::create()
    ->add_aKeyValue(DTKeyValue::create()->set_sKey('foo')->set_sValue('bar'))
);
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="event-bind"></a>
## `\MVC\Event::bind()`

with `bind` you create a Listener to an expected event;
you _bind_ a closure to the expected event.

_Syntax_
~~~
\MVC\Event::bind('eventName', {closure} );
~~~

_Example: bind a closure to the Event "foo"_
~~~php
\MVC\Event::bind('foo', function(\MVC\DataType\DTArrayObject $oDTArrayObject) {
    \MVC\Debug:info($oDTArrayObject);
});
~~~
- If the event 'foo' is triggered, the closure will be executed: the content of `$oDTArrayObject` will be displayed on screen

_Example: bind a closure to a concrete Controller::method_ 
~~~php
\MVC\Event::bind('\{module}\Controller\Index::foo', function (\MVC\DataType\DTArrayObject $oDTArrayObject) {
    \MVC\Debug:info($oDTArrayObject);
});
~~~
- If the method `\{module}\Controller\Index::foo` is being called, the closure will be executed: the content of `$oDTArrayObject` will be displayed on screen
- ⚠ Make sure to write the event name as the method was a static one, even it is not.

------------------------------------------------------------------------------------------------------------------------

<a id="processBindConfigStack"></a>
### `\MVC\Event::processBindConfigStack()` - Bind to Events via config Stack

Instead of writing one `bind` command expressure after the other you can make use of array notation and use `Event::processBindConfigStack`.

This way you can note bondings to multiple Events at once and you can even note multiple closures for each event.

This reduces complexity and improves readability.

_Example: using config Stack to bind closures to Events_  
~~~php
<?php
\MVC\Event::processBindConfigStack([ // Bondings to 2 Events
    '\{module}\Controller\Index::foo' => [ // 2 Closures bonded to this Event
        function (\MVC\DataType\DTArrayObject $oDTArrayObject) {      
            \MVC\Debug:info($oDTArrayObject);
        },
        function (\MVC\DataType\DTArrayObject $oDTArrayObject) {      
            \MVC\Log:write($oDTArrayObject, 'debug.log');
        }
    ],
    '\{module}\Controller\Index::bar' => [ // 1 Closure bonded to this Event
        function (\MVC\DataType\DTArrayObject $oDTArrayObject) {      
            \MVC\Log:write($oDTArrayObject, 'debug.log');
        }
    ],    
]);
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="event-delete"></a>
## `\MVC\Event::delete()`

delete one or all events. 

⚠ If this parameter not is set, *all* events are going to be deleted.

_Example: delete the certain Event named `foo.bar`_  
~~~php
\MVC\Event::delete('foo.bar');
~~~

_Example: delete *all* Events_  
~~~php
\MVC\Event::delete();
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Early-Event-bondings"></a>
## Early Event bondings

These bondings are executed right after all Configurations have been loaded. No essential classes of myMVC have been loaded or processed at this point yet. 

So this is the perfect place to influence the behavior of the starting application.

_Path to module's early event bonding files_    
~~~
module/{module}/etc/config/event/
~~~

_Example: log current request object to debug.log when on develop environment_    
~~~php
<?php

\MVC\Event::processBindConfigStack([
    'mvc.request.getCurrentRequest.after' => [
        function (\MVC\DataType\DTArrayObject $oDTArrayObject) {
            // get request object
            $oDTRequestCurrent = $oDTArrayObject->getDTKeyValueByKey('oDTRequestCurrent')->get_sValue();

            if ('develop' === \MVC\Config::get_MVC_ENV())
            {
                \MVC\Log::write($oDTRequestCurrent, 'debug.log');
            }
        },
    ],
]);
~~~

You can write your own event bondings into an already existing file.

You can also create your own php file in that folder and note your event bondings, no problem.  
⚠ but consider the loading order of the files, which is a-z.


------------------------------------------------------------------------------------------------------------------------

<a id="myMVCStandardEvents"></a>
## myMVC Standard Events 

| Event                                                 | `Event::bind` perforemd in             | `Event::run` located in                                                                                             |
|-------------------------------------------------------|----------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| policy.index.requestMethodHasToMatchRouteMethod.after | modules/{module}/etc/event/policy.php  | modules/{module}/Policy/Index.php                                                                                   |
| mvc.application.setSession.before                     | modules/{module}/etc/event/default.php | application/library/MVC/Application.php                                                                             |
| mvc.application.setSession.after                      |                                        | application/library/MVC/Application.php                                                                             |
| mvc.application.construct.after                       |                                        | application/library/MVC/Application.php                                                                             |
| mvc.application.destruct.before                       |                                        | application/library/MVC/Application.php                                                                             |
| mvc.controller.init.before                            | application/library/MVC/Request.php    | application/library/MVC/Controller.php                                                                              |
| mvc.controller.construct.after `@deprecated`          |                                        | application/library/MVC/Controller.php                                                                              |
| mvc.controller.init.after                             |                                        | application/library/MVC/Controller.php                                                                              |
| mvc.runTargetClassPreconstruct.after `@deprecated`    |                                        | application/library/MVC/Controller.php                                                                              |
| mvc.controller.runTargetClassPreconstruct.after       |                                        | application/library/MVC/Controller.php                                                                              |
| mvc.controller.destruct.before                        |                                        | application/library/MVC/Controller.php                                                                              |
| mvc.error                                             |                                        | application/library/MVC/Controller.php<br>application/library/MVC/Request.php<br>application/library/MVC/Policy.php |
| mvc.policy.init.before                                |                                        | application/library/MVC/Policy.php                                                                                  |
| mvc.policy.init.after                                 |                                        | application/library/MVC/Policy.php                                                                                  |
| mvc.policy.set.before                                 |                                        | application/library/MVC/Policy.php                                                                                  |
| mvc.policy.set.after                                  |                                        | application/library/MVC/Policy.php                                                                                  |
| mvc.policy.unset.before                               |                                        | application/library/MVC/Policy.php                                                                                  |
| mvc.policy.unset.after                                |                                        | application/library/MVC/Policy.php                                                                                  |
| mvc.policy.apply.before                               |                                        | application/library/MVC/Policy.php                                                                                  |
| mvc.policy.apply.execute                              |                                        | application/library/MVC/Policy.php                                                                                  |
| mvc.reflex.reflect.before                             |                                        | application/library/MVC/Reflex.php                                                                                  |
| mvc.reflex.reflect.targetObject.before                | modules/{module}/etc/event/default.php | application/library/MVC/Reflex.php                                                                                  |
| mvc.reflex.reflect.targetObject.after                 | modules/{module}/etc/event/default.php | application/library/MVC/Reflex.php                                                                                  |
| mvc.reflex.destruct.before                            |                                        | application/library/MVC/Reflex.php                                                                                  |
| mvc.route.init `@deprecated`                          |                                        | application/library/MVC/Route.php                                                                                   |
| mvc.route.init.before                                 |                                        | application/library/MVC/Route.php                                                                                   |
| mvc.route.init.after                                  |                                        | application/library/MVC/Route.php                                                                                   |
| $sControllerClassName . '::' . $sMethod               |                                        | application/library/MVC/Reflex.php                                                                                  |
| mvc.request.getCurrentRequest.after                   |                                        | application/library/MVC/Request.php                                                                                 |
| mvc.request.redirect                                  |                                        | application/library/MVC/Request.php                                                                                 |
| mvc.view.render.before                                | application/library/MVC/InfoTool.php   | application/library/MVC/View.php                                                                                    |
| mvc.view.renderString.before                          |                                        | application/library/MVC/View.php                                                                                    |
| mvc.view.renderString.after                           |                                        | application/library/MVC/View.php                                                                                    |
| mvc.view.render.after                                 |                                        | application/library/MVC/View.php                                                                                    |
| mvc.debug.stop.after                                  | modules/Doc/etc/event/default.php      | application/library/MVC/Debug.php                                                                                   |
| mvc.lock.create                                       |                                        | application/library/MVC/Lock.php                                                                                    |
| mvc.view.echoOut.off                                  | application/library/MVC/View.php       |                                                                                                                     |
| mvc.view.echoOut.on                                   | application/library/MVC/View.php       |                                                                                                                     |
| mvc.view.render.off                                   | application/library/MVC/View.php       |                                                                                                                     |
| mvc.view.render.on                                    | application/library/MVC/View.php       |                                                                                                                     |
