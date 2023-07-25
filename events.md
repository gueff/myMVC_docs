
# Events

- [Registering Event Listeners](#registering-event-listeners)
- [`\MVC\Event::run()`](#event-run)
- [`\MVC\Event::bind()`](#event-bind)
  - [`\MVC\Event::processBindConfigStack()` - Bind to Events via config Stack](#processBindConfigStack)
- [`\MVC\Event::delete()`](#event-delete) 
- [myMVC Standard Events](#myMVCStandardEvents)

------------------------------------------------------------------------------------------------------------------------

<a id="registering-event-listeners"></a>
## Registering Event Listeners

_Location where to write Event Listeners_  
~~~
module/{module}/etc/config/event/
~~~

Listeners written here are executed right after all Configurations have been loaded. No essential classes of myMVC have been loaded or processed at this point yet.

So this is the perfect place to influence the behavior of the starting application.

_Example: log current request object to debug.log when on develop environment_
~~~php
<?php

\MVC\Event::processBindConfigStack([

    'mvc.request.getCurrentRequest.after' => [ // Event
    
        function (\MVC\DataType\DTArrayObject $oDTArrayObject) { // Closure
        
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

And you may add as many events as your application requires.

You can also create your own php file in that folder and note your event bondings, no problem.  
⚠ but consider the loading order of the files, which is a-z.


------------------------------------------------------------------------------------------------------------------------

<a id="event-run"></a>
## `\MVC\Event::run()` 

with `run` you initialize an event; bonded Closures to the event name get executed in order.

_Syntax_
~~~
\MVC\Event::run('eventName', {mixed} );
~~~

_Example: Run a simple Event_
~~~php
\MVC\Event::run('foo');
~~~

_Example: pass a value to the event_  
~~~php
\MVC\Event::run('foo', 123);
~~~

_Example: pass an `DTArrayObject` Object with Infos_  
~~~php
\MVC\Event::run('foo', DTArrayObject::create()
    ->add_aKeyValue(
        DTKeyValue::create()->set_sKey('foo')->set_sValue('bar')
    )
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

_Syntax_  
~~~
\MVC\Event::processBindConfigStack([ '{EventName}' => [ {Closure}, ], ]);
~~~

This way you can note bondings to multiple Events at once and you can even note multiple closures for each event.

This reduces complexity and improves readability.

_Example: using config Stack to bind closures to Events_  
~~~php
<?php
\MVC\Event::processBindConfigStack([ // Bondings to 2 Events

    '\Foo\Controller\Index::foo' => [ // 2 Closures bonded to this Event
    
        function (\MVC\DataType\DTArrayObject $oDTArrayObject) {      
            \MVC\Debug:info($oDTArrayObject);
        },
        function (\MVC\DataType\DTArrayObject $oDTArrayObject) {      
            \MVC\Log:write($oDTArrayObject, 'debug.log');
        }
    ],
    '\Foo\Controller\Index::bar' => [ // 1 Closure bonded to this Event
    
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

<a id="myMVCStandardEvents"></a>
## myMVC Standard Events 

| Event Name                                            | `Event::bind` perforemd in                                   | `Event::run` located in                                                                                 | value passed                                                     |
|-------------------------------------------------------|--------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|
| mvc.event.init.after                                  |                                                              | `\MVC\Event::init`                                                                                      |                                                                  |
| policy.index.requestMethodHasToMatchRouteMethod.after | modules/{module}/etc/event/policy.php                        | `\{module}\Policy\Index::requestMethodHasToMatchRouteMethod`                                            | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.application.setSession.before                     | modules/{module}/etc/event/default.php                       | `\MVC\Application::initSession`                                                                         |                                                                  |
| mvc.application.setSession.after                      |                                                              | `\MVC\Application::initSession`                                                                         | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.application.construct.after                       |                                                              | `\MVC\Application::__construct`                                                                         |                                                                  |
| mvc.application.destruct.before                       |                                                              | `\MVC\Application::__destruct`                                                                          | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.controller.init.before                            | `\MVC\Request::getCurrentRequest`                            | `\MVC\Controller::init`                                                                                 | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.controller.init.after                             |                                                              | `\MVC\Controller::init`                                                                                 | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.controller.runTargetClassPreconstruct.after       |                                                              | `\MVC\Controller::runTargetClassPreconstruct`                                                           | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.controller.destruct.before                        |                                                              | `\MVC\Controller::__destruct`                                                                           | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.error                                             | `\MVC\Error::init`<br>modules/{module}/etc/event/default.php | `\MVC\Controller::runTargetClassPreconstruct`<br>`\MVC\Request::getUriProtocol`<br>`\MVC\Policy::apply` | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.policy.init.before                                |                                                              | `\MVC\Policy::init`                                                                                     |                                                                  |
| mvc.policy.init.after                                 |                                                              | `\MVC\Policy::init`                                                                                     |                                                                  |
| mvc.policy.set.before                                 |                                                              | `\MVC\Policy::set`                                                                                      | `array $aPolicy` _all declared policies_                         |
| mvc.policy.set.after                                  |                                                              | `\MVC\Policy::set`                                                                                      | `array $aPolicy` _all declared policies_                         |
| mvc.policy.unset.before                               |                                                              | `\MVC\Policy::unset`                                                                                    | `array $aPolicy` _all declared policies_                         |
| mvc.policy.unset.after                                |                                                              | `\MVC\Policy::unset`                                                                                    | `array $aPolicy` _all declared policies_                         |
| mvc.policy.apply.before                               |                                                              | `\MVC\Policy::apply`                                                                                    | `array $aPolicy` _matching policy rules on the current request_  |
| mvc.policy.apply.execute                              |                                                              | `\MVC\Policy::apply`                                                                                    | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.reflex.reflect.before                             |                                                              | `\MVC\Reflex::reflect`                                                                                  | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.reflex.reflect.targetObject.before                | modules/{module}/etc/event/default.php                       | `\MVC\Reflex::reflect`                                                                                  | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.reflex.reflect.targetObject.after                 | modules/{module}/etc/event/default.php                       | `\MVC\Reflex::reflect`                                                                                  | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.reflex.destruct.before                            |                                                              | `\MVC\Reflex::__destruct`                                                                               | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.route.init.before                                 |                                                              | `\MVC\Route::init`                                                                                      |                                                                  |
| mvc.route.init.after                                  |                                                              | `\MVC\Route::init`                                                                                      |                                                                  |
| `$sControllerClassName :: $sMethod`                   |                                                              | `\MVC\Reflex::reflect`                                                                                  |                                                                  |
| mvc.request.getCurrentRequest.after                   |                                                              | `\MVC\Request::getCurrentRequest`                                                                       | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.request.redirect                                  |                                                              | `\MVC\Request::redirect`                                                                                | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.view.render.before                                | `\MVC\InfoTool::__construct`                                 | `\MVC\View::render`                                                                                     | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.view.renderString.before                          |                                                              | `\MVC\View::renderString`                                                                               | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.view.renderString.after                           |                                                              | `\MVC\View::renderString`                                                                               | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.view.render.after                                 |                                                              | `\MVC\View::render`                                                                                     | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.debug.stop.after                                  | modules/{module}/etc/event/default.php                       | `\MVC\Debug::stop`                                                                                      | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.lock.create                                       |                                                              | `\MVC\Lock::create`                                                                                     | `\MVC\DataType\DTArrayObject $oDTArrayObject`                    |
| mvc.view.echoOut.off                                  | `\MVC\View::__construct`                                     |                                                                                                         |                                                                  |
| mvc.view.echoOut.on                                   | `\MVC\View::__construct`                                     |                                                                                                         |                                                                  |
| mvc.view.render.off                                   | `\MVC\View::__construct`                                     |                                                                                                         |                                                                  |
| mvc.view.render.on                                    | `\MVC\View::__construct`                                     |                                                                                                         |                                                                  |
