
# Policy 

- [Set a Policy Rule](#Set-a-Policy-Rule)
- [Unset Policy Rules](#Unset-Policy-Rules)
- [Write methods to be executed by policy](#Write-methods-to-run)

------------------------------------------------------------------------------------------------------------------------
<a id="Set-a-Policy-Rule"></a>
## Set a Policy Rule

Policies are bonded to a any/certain Controller::method.  
And they are executed after all Routes are initialized by myMVC.

_Path to module's policy configuration file_
~~~
module/{module}/etc/config/policy/policy.php
~~~

Inside this file you define your policies.

**Examples** - assuming module is "Foo"

_Set a Rule to **any** method of a controller_   
~~~php
\MVC\Policy::set(
    '\Foo\Controller\Index', '*',
    '\Foo\Policy\Index::requestMethodHasToMatchRouteMethod'
);
~~~
- when any (`*`) method of controller `\Foo\Controller\Index` is being called
- execute `\Foo\Policy\Index::requestMethodHasToMatchRouteMethod`

_Set a Rule to a **certain** method of a controller_
~~~php
\MVC\Policy::set(
    '\Foo\Controller\Index', 'index',
    '\Foo\Policy\Index::requestMethodHasToMatchRouteMethod'
);
~~~
- when method `index` of controller `\Foo\Controller\Index` is being called
- execute `\Foo\Policy\Index::requestMethodHasToMatchRouteMethod`

------------------------------------------------------------------------------------------------------------------------
<a id="Unset-Policy-Rules"></a>
## Unset Policy Rules

**Examples** - assuming module is "Foo"

_Unset a certain target method_  
~~~php
Policy::unset('\Foo\Controller\Index', 'index', '\Foo\Policy\Index::requestMethodHasToMatchRouteMethod');
~~~

_Unset all target methods set to controller '\Foo\Controller\Index' and method 'index'_  
~~~php
Policy::unset('\Foo\Controller\Index', 'index');
~~~

_Unset all rules set to controller '\Foo\Controller\Index'_
~~~php
Policy::unset('\Foo\Controller\Index');
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