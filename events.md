# Events

- [Events](#Events)
	- [EventNames](#EventNames)
	- [Class::method](#Class_method)
- [`bind` and `run`](#BIND_and_RUN)
- [myMVC Standard Events](#myMVCStandardEvents)
- [Events myMVC is listening to](#myMVCEventsListening)
- [Helper Methods](#Helper_Methods)

---

<a id="Events"></a>
## Events 

You can listen to Events in 2 ways:

1. Event Names
2. Class::method


<a id="EventNames"></a>
### 1. Event Names 

you can easily run an individual event.  
Just note the event name and pass an `DTArrayObject` Object with Infos.

_Example_  
~~~php
Event::RUN('module.controller.method.action',
	DTArrayObject::create()
		->add_aKeyValue(
			DTKeyValue::create()->set_sKey('_aQueryVar')->set_sValue($this->_aQueryVar)
		)
		->add_aKeyValue(
			DTKeyValue::create()->set_sKey('_sRequestUri')->set_sValue($this->_sRequestUri)
		)
);
~~~

<a id="Class_method"></a>
### 2. Class::method 

Using a Concrete Controller::method  of User's Application. Instead of an Event-Name to listen to you can always address a certain Method of a Controller. That gives you even more flexibility.

Example: If you want to listen to when the Method "\Standard\Controller\Index::home" is being called, simply note the method as the event name. Make sure to write it as the method was a static one, even it is not.

_Example_  
~~~php
/*
 * redirect if explicitly if the method "foo" is requested
 */
\MVC\Event::BIND ('\Module\Controller\Index::foo', function ($oObject) {
    \MVC\Request::REDIRECT('/');
});
~~~

<a id="BIND_and_RUN"></a>
## `bind` and `run`

first you `bind` to an event, then you `run` the event. 


### `bind`

with `bind` you create a Listener to an expected event;
you _bind_ a closure to the expected event.

_syntax_  
~~~
\MVC\Event::bind ('eventName', {closure} );
~~~

_example: bind a closure to the Event 'foo'_  
~~~php
\MVC\Event::bind ('foo', function($oDTArrayObject) {
        \MVC\Helper::DISPLAY ($oDTArrayObject);
});
~~~
- if the event 'foo' is triggered, the closure will be executed: the content of `$oDTArrayObject` will be displayed on screen

### `run`

with `run` you trigger an event.

_Run an Event_  
~~~php
\MVC\Event::run ('foo');
~~~

_Run an Event and deploy a Package which could be read inside the Bind/Closure_   
~~~php
$oDTArrayObject = DTArrayObject::create()->add_aKeyValue(
	DTKeyValue::create()->set_sKey('foo')->set_sValue('bar')
);
\MVC\Event::run ('foo', $oDTArrayObject);
~~~


<a id="myMVCStandardEvents"></a>
## myMVC Standard Events 

In chronological order;  
The path shows where the event is going to be called (RUN)

- `mvc.request.saveRequest.after`

	~~~
	MVC/Request.php

	request is prepared for usage
	~~~
	~~~
	Event::RUN('mvc.request.saveRequest.after',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('_aQueryVar')->set_sValue($this->_aQueryVar)
			)
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('_sRequestUri')->set_sValue($this->_sRequestUri)
			)
	);
	~~~
- `mvc.request.prepareQueryVarsForUsage.after`

	~~~
	MVC/Request.php
	~~~
	~~~
	Event::RUN('mvc.request.prepareQueryVarsForUsage.after',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('_aQueryVar')->set_sValue($this->_aQueryVar)
			)
	);
	~~~
- `mvc.runTargetClassPreconstruct.after`

	~~~
	MVC/Application.php

	the "__preconstruct()" method inside the requested Controller has been run
	~~~
	~~~
	Event::RUN ('mvc.runTargetClassPreconstruct.after',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('sClass')->set_sValue($sClass)
			)
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('sMethod')->set_sValue($sMethod)
			)
	);
	~~~
- `mvc.application.setSession.before`

	~~~
	MVC/Application.php
	~~~
	~~~
	Event::RUN ('mvc.application.setSession.before', DTArrayObject::create());
	~~~
- `mvc.application.setSession.after`

	~~~
	MVC/Application.php
	Session Object is built and copied to the registry
	~~~
	~~~
	Event::RUN ('mvc.application.setSession.after',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('oSession')->set_sValue($oSession)
			)
	);
	~~~
- `mvc.policy.apply.execute`

	~~~
	MVC/Policy.php
	~~~
	~~~
	Event::RUN ('mvc.policy.apply.execute',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('bSuccess')->set_sValue($bSuccess)
			)
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('sPolicy')->set_sValue($sPolicy)
			)
	);
	~~~
- `mvc.controller.init.before`

	~~~
	MVC/Controller.php
	~~~
	~~~
	Event::RUN ('mvc.controller.init.before', DTArrayObject::create());
	~~~
- `mvc.reflex.reflect.before`

	~~~
	MVC/Reflex.php
	~~~
	~~~
	Event::RUN ('mvc.reflex.reflect.before',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('aQueryArray')->set_sValue($aQueryArray)
			)
	);
	~~~
- `mvc.reflex.reflect.targetObject.before`

	~~~
	MVC/Reflex.php

	contains the target Class as the already instanciated object
	which for sure could be accessed
	This event is called immediatly before the target method will be called
	~~~
	~~~
	// run an event and store the object of the target class within
	Event::RUN ('mvc.reflex.reflect.targetObject.before',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('oReflectionObject')->set_sValue($oReflectionObject)
			)
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('sMethod')->set_sValue($sMethod)
			)
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('sArgs')->set_sValue($sArgs)
			)
	);
	~~~
- `$sControllerClassName . '::' . $sMethod`

	~~~
	MVC/Reflex.php

	e.g. "\Standard\Controller\Index::home"
	run an event which KEY is
			Class::method
	of the requested Target
	and store the object of the target class within
	~~~
	~~~
	// run an event which KEY is
	//		Class::method 
	// of the requested Target
	// and store the object of the target class within
	Event::RUN ($sControllerClassName . '::' . $sMethod,
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('oReflectionObject')->set_sValue($oReflectionObject)
			)
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('sMethod')->set_sValue($sMethod)
			)
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('sArgs')->set_sValue($sArgs)
			)
	);
	~~~
- `mvc.reflex.reflect.targetObject.after`

	~~~
	MVC/Reflex.php

	contains the target Class as the already instanciated object
	which for sure could be accessed
	This event is called immediatly after the target method was called
	~~~
	~~~
	Event::RUN ('mvc.reflex.reflect.targetObject.after',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('oReflectionObject')->set_sValue($oReflectionObject)
			)
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('sMethod')->set_sValue($sMethod)
			)
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('sArgs')->set_sValue($sArgs)
			)
	);
	~~~
- `mvc.controller.construct.after`

	~~~
	MVC/Controller.php
	~~~
	~~~
	Event::RUN ('mvc.controller.construct.after',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('bSuccess')->set_sValue($bSuccess)
			)
	);
	~~~
- `mvc.view.render.before`

	~~~
	MVC/View.php
	~~~
	~~~
	Event::RUN ('mvc.view.render.before',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('oView')->set_sValue($this)
			)
	);
	~~~
- `mvc.view.renderString.before`

	~~~
	MVC/View.php
	~~~
	~~~
	Event::RUN ('mvc.view.renderString.before',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('sTemplateString')->set_sValue($sTemplateString)
			)
	);
	~~~
- `mvc.view.renderString.after`

	~~~
	MVC/View.php
	~~~
	~~~
	Event::RUN ('mvc.view.renderString.after',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('sTemplateString')->set_sValue($sTemplateString)
			)
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('sRendered')->set_sValue($sRendered)
			)
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('bEchoOut')->set_sValue(self::$_bEchoOut)
			)
	);
	~~~
- `mvc.view.render.after`

	~~~
	MVC/View.php
	~~~
	~~~
	Event::RUN ('mvc.view.render.after',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('oView')->set_sValue($this)
			)
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('sTemplate')->set_sValue($sTemplate)
			)
	);
	~~~
- `mvc.reflex.destruct.before`

	~~~
	MVC/Reflex.php
	~~~
	~~~
	Event::RUN ('mvc.reflex.destruct.before',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('oReflex')->set_sValue($this)
			)
	);
	~~~
- `mvc.controller.destruct.before`

	~~~
	MVC/Controller.php
	~~~
	~~~
	Event::RUN ('mvc.controller.destruct.before',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('oController')->set_sValue($this)
			)
	);
	~~~
- `mvc.application.construct.after`

	~~~
	MVC/Application.php
	~~~
	~~~
	Event::run ('mvc.application.construct.after', DTArrayObject::create());
	~~~
- `mvc.application.destruct.before`

	~~~
	MVC/Application.php
	~~~
	~~~
	Event::RUN ('mvc.application.destruct.before',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('oController')->set_sValue($this)
			)
	);
	~~~


**other**


- `mvc.helper.stop.after`

	~~~
	MVC/Helper.php
	~~~
	~~~
	Event::RUN ('mvc.helper.stop.after',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('aBacktrace')->set_sValue($aBacktrace)
			)
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('mData')->set_sValue($sEcho)
			)
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('bOccurrence')->set_sValue($bShowWhereStop)
			)
	);
	~~~
- `mvc.lock.create`

	~~~
	MVC/Lock.php
	~~~
	~~~
	Event::RUN('mvc.lock.create', DTArrayObject::create()
		->add_aKeyValue(DTKeyValue::create()->set_sKey('aBacktrace')->set_sValue($aBacktrace))
		->add_aKeyValue(DTKeyValue::create()->set_sKey('bLocked')->set_sValue(true))
		->add_aKeyValue(DTKeyValue::create()->set_sKey('sFile')->set_sValue($sFile))
	);
	~~~
- `mvc.error`

	~~~
	MVC/Policy.php
	~~~
	~~~
	Event::RUN ('mvc.error',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('sMessage')->set_sValue("Policy could not be executed: " . $sPolicy)
			)
	);
	~~~
	~~~
	MVC/Helper.php
	~~~
	~~~
	\MVC\Event::RUN('mvc.error',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('sMessage')->set_sValue('could not detect protocol of requested page.')
			)
	);	
	~~~
	~~~
	MVC/Application.php
	~~~
	~~~
	Event::RUN ('mvc.error',
		DTArrayObject::create()
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('iLevel')->set_sValue(1)
			)
			->add_aKeyValue(
				DTKeyValue::create()->set_sKey('sMessage')->set_sValue(__FILE__ . ', ' . __LINE__ . "\t" . 'Class does not exist: `' . $sClass . '`')
			)
	);
	~~~	


<a id="myMVCEventsListening"></a>
## Events myMVC is listening to 

- `mvc.view.echoOut.off`

	~~~
	MVC/View.php

	disables the echo out of the rendered view template
	~~~
	~~~
	\MVC\Event::BIND('mvc.view.echoOut.off', function () {
		
		MVC_View::$bEchoOut = false;
	});	
	~~~
- `mvc.view.echoOut.on`

	~~~
	MVC/View.php

	enables the echo out of the rendered view template (default=On)
	~~~
	~~~
	\MVC\Event::BIND('mvc.view.echoOut.on', function () {
		
		MVC_View::$bEchoOut = true;
	});	
	~~~
	

<a id="Helper_Methods"></a>
## Helper Methods 

_Detect a Closure_  
~~~php
\MVC\Helper::isClosure($oExpectedClosure))
~~~

_Call a Closure_  
~~~php
call_user_func ($oPackage)
~~~