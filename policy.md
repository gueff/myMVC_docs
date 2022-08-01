Policy Rules are bonded to a specific `Controller::Method()`.

If a rule is detected for the requested method, the static Methods which are defined below will be executed then.

This allows you to seperate Access Control from your Controllers.


_Example_  
Create a config file "policy.php"  
- globally in `/config/` folder
- **or** in your module's config folder `/modules/{moduleName}/etc/config/{moduleName}/config/`

See  [/configuration/](/configuration/) for more Information.  
Inside this file you define your policies.

~~~php
<?php

$aConfig['MVC_POLICY'] = array(

    // The matching Class
    // (do no use leading slashes for Controller Names)
    'Foo\\Controller\\Index' => array(

        // the matching method
        'home' => array(

                // What to call
                // (do no use leading slashes for Controller Names)
                'Foo\\Policy\\Example::test1'
            ,	'Foo\\Policy\\Example::test2'
        )
    )
);
~~~