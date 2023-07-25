
# Policy

- [Enabling Policy Rules](#writing-policy-rules)
  - [Set a single Rule](#set-a-single-rule)
  - [Setting multiple Rules](#setting-multiple-rules)
  - [Bind a Policy Rule to a Route](#Bind-a-Policy-Rule-to-a-Route)
- [Disabling Policy Rules](#disabling-policy-rules)
  - [Unset a single Rule](#unset-a-single-rule)
  - [Unset multiple Rules](#unset-multiple-rules)
  - [Unbind a Policy Rule from a Route](#unbind-a-policy-rule-from-a-route)
- [Write methods to be executed by policy](#Write-methods-to-run)

------------------------------------------------------------------------------------------------------------------------

<a id="writing-policy-rules"></a>
## Enabling Policy Rules

Policies are bonded to a any/certain Controller::method.  
And they are executed after all Routes are initialized by myMVC.

_Path to module's policy configuration files_
~~~
module/{module}/etc/config/policy/*.php
~~~
~~~
module/{module}/etc/config/policy/policy.php
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="set-a-single-rule"></a>
### Set a single Rule

_(assuming module is "Foo")_

_Set a single Rule to **any** (`*`) method of a controller_
~~~php
\MVC\Policy::set(
    '\Foo\Controller\Index', 
    '*', 
    '\Foo\Policy\Index::requestMethodHasToMatchRouteMethod'    
);
~~~
- when any (`*`) method of controller `\Foo\Controller\Index` is being called
- execute `\Foo\Policy\Index::requestMethodHasToMatchRouteMethod`

_Set a single Rule to a **certain** method of a controller_
~~~php
\MVC\Policy::set(
    '\Foo\Controller\Index', 
    'index', 
    '\Foo\Policy\Index::requestMethodHasToMatchRouteMethod'
);
~~~
- when method `index` of controller `\Foo\Controller\Index` is being called
- execute `\Foo\Policy\Index::requestMethodHasToMatchRouteMethod`

<a id="setting-multiple-rules"></a>
### Setting multiple Rules

simply wrap the rules into an array.

~~~php
\MVC\Policy::set(
    '\Foo\Controller\Index', 
    '*', [
        '\Foo\Policy\Index::foo',
        '\Foo\Policy\Index::bar',
        '\Foo\Policy\Index::baz',
    ]
);
~~~

<a id="Bind-a-Policy-Rule-to-a-Route"></a>
### Bind a Policy Rule to a Route

You can also bind a policy to a certain Route.

_Example: bind policies `checkUserRights` and `isAdmin` to Route `/foo/bar/`_
~~~php
\MVC\Policy::bindOnRoute(
    \MVC\Route::$aRoute['/foo/bar/'], [
        '\Blg\Policy\Index::checkUserRights',
        '\Blg\Policy\Index::isAdmin'
    ]
);
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="disabling-policy-rules"></a>
## Disabling Policy Rules

<a id="unset-a-single-rule"></a>
### Unset a single Rule

assuming module is "Foo"

_Unset the single rule `'\Foo\Policy\Index::requestMethodHasToMatchRouteMethod'` set to controller `\Foo\Controller\Index` and method `index`_
~~~php
Policy::unset(
    '\Foo\Controller\Index', 
    'index', 
    '\Foo\Policy\Index::requestMethodHasToMatchRouteMethod'    
);
~~~

<a id="unset-multiple-rules"></a>
### Unset multiple Rules

_Unset certain, multiple rules set to controller `\Foo\Controller\Index` and method `index`_
~~~php
\MVC\Policy::unset(
    '\Foo\Controller\Index', 
    'index', [
        '\Foo\Policy\Index::foo',
        '\Foo\Policy\Index::bar',
        '\Foo\Policy\Index::baz',
    ]
);
~~~

_Unset all rules set to controller `\Foo\Controller\Index` and method `index`_
~~~php
Policy::unset(
    '\Foo\Controller\Index', 
    'index'
);
~~~

_Unset all rules set to controller '\Foo\Controller\Index'_
~~~php
Policy::unset(
    '\Foo\Controller\Index'
);
~~~

<a id="unbind-a-policy-rule-from-a-route"></a>
### Unbind a Policy Rule from a Route

_Unbind a certain policy rule_
~~~php
\MVC\Policy::unbindRoute(
    \MVC\Route::$aRoute['/foo/bar/'], [
        '\Blg\Policy\Index::checkUserRights'
    ]
);
~~~

_Unbind all policy rules bonded to a route_
~~~php
\MVC\Policy::unbindRoute(
    \MVC\Route::$aRoute['/foo/bar/']
);
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Write-methods-to-run"></a>
## Write methods to be executed by policy

For a better overview, you note the required class::method not in "module/{module}/Model/" but in a separate folder.

_Path to module's policy folder_
~~~
module/{module}/Policy/
~~~

_Example: file `module/Foo/Policy/Index.php`_
~~~php
<?php
/**
 * Index.php
 *
 * @package myMVC
 * @copyright ueffing.net
 * @author Guido K.B.W. Üffing <info@ueffing.net>
 * @license GNU GENERAL PUBLIC LICENSE Version 3. See application/doc/COPYING
 */

/**
 * @name $FooPolicy
 */
namespace Foo\Policy;

use MVC\DataType\DTArrayObject;
use MVC\DataType\DTKeyValue;
use MVC\Event;

/**
 * Index
 */
class Index
{
    /**
     * @return void
     * @throws \ReflectionException
     */
	public static function requestMethodHasToMatchRouteMethod ()
	{
        $oDTArrayObject = DTArrayObject::create()
            ->add_aKeyValue(DTKeyValue::create()->set_sKey('sRequestmethod')->set_sValue(\MVC\Request::getCurrentRequest()->get_requestmethod()))
            ->add_aKeyValue(DTKeyValue::create()->set_sKey('aMethodsAssigned')->set_sValue(\MVC\Route::getCurrent()->get_methodsAssigned()))
            ->add_aKeyValue(DTKeyValue::create()->set_sKey('bGrant')->set_sValue(false))
            ->add_aKeyValue(DTKeyValue::create()->set_sKey('sMessage')->set_sValue('access denied'))
            ->add_aKeyValue(DTKeyValue::create()->set_sKey('sMethod')->set_sValue(__METHOD__))
            ->add_aKeyValue(DTKeyValue::create()->set_sKey('sRedirect')->set_sValue('/404/'))
            ->add_aKeyValue(DTKeyValue::create()->set_sKey('sClosure')->set_sValue(
                function(\MVC\DataType\DTArrayObject $oDTArrayObject) {
                    \MVC\Request::redirect($oDTArrayObject->getDTKeyValueByKey('sRedirect')->get_sValue());
                }
            ))
        ;

        // grant access
        if (
            // Route is of type ANY
            '*' === \MVC\Route::getCurrent()->get_method()
            OR
            // Request and Route methods do match
            true === in_array(\MVC\Request::getCurrentRequest()->get_requestmethod(), \MVC\Route::getCurrent()->get_methodsAssigned(), true)
        )
        {
            $oDTArrayObject->setDTKeyValueByKey(DTKeyValue::create()->set_sKey('bGrant')->set_sValue(true));
            $oDTArrayObject->setDTKeyValueByKey(DTKeyValue::create()->set_sKey('sMessage')->set_sValue('access granted'));
        }

        // give any middleware option to modify $oDTArrayObject
        Event::run('policy.index.requestMethodHasToMatchRouteMethod.after', $oDTArrayObject);

        // deny access; call closure
        if (false === $oDTArrayObject->getDTKeyValueByKey('bGrant')->get_sValue())
        {
            call_user_func($oDTArrayObject->getDTKeyValueByKey('sClosure')->get_sValue(), $oDTArrayObject);
        }
	}
}
~~~
