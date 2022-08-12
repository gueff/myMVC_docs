
# Generating DataType Classes

with this Generator you can easily generate DataType Classes. Consider those Classes more than a Storage than a Logic Element.

You can easily define what name the class and which namespace it should have, which properties or constants it should provide.

After you defined it, just run the Generator and it will create the Class for you.


<!--
## Overview

- [Instantiation](#Instantiation)
- [Init with Config: Object](#init_object)
    - [Config `$oDTConfig`](#object_config)
- [Init with Config: array](#init_array)
    - [Config `$aDataTypeConfig`](#array_config)
- [Hint](#HInt)
- [Valid types](#Valid_types)
-->

<!--
<a id="Instantiation"></a>
## Instantiation

_PHP auto detect compatible_
~~~php
$oDTGenerator = \MVC\Generator\DataType::create();
~~~

_PHP 5.3 compatible_
~~~php
$oDTGenerator = \MVC\Generator\DataType::create(53);
~~~

_PHP 7 and up compatible_
~~~php
$oDTGenerator = \MVC\Generator\DataType::create(7);
~~~

_PHP 7.3 compatible_
~~~php
$oDTGenerator = \MVC\Generator\DataType::create(73);
~~~

<a id="init_object"></a>
## Init with Config: Object
~~~php
$oDTGenerator = \MVC\Generator\DataType::create(56)->initConfigObject($oDTConfig);
~~~
-->

<a id="object_config"></a>
### Configuration as Object

_Example_  
~~~php
// config
$oDTConfig = \MVC\DataType\DTConfig::create()
    ->set_dir(\MVC\Config::get_MVC_MODULES() . '/Foo/DataType/')
    ->set_unlinkDir(false)
    ->add_DTClass(

        \MVC\DataType\DTClass::create()
            ->set_name('DTFoo')
            ->set_file('DTFoo.php')
            ->set_namespace('Foo\DataType')
            ->set_createHelperMethods(true)
            ->add_DTConstant(
                \MVC\DataType\DTConstant::create()
                    ->set_key('FOO')
                    ->set_value('"BAR"')
                    ->set_visibility('public')
            )
            ->add_DTProperty(
                \MVC\DataType\DTProperty::create()
                    ->set_key('bSuccess')
                    ->set_var('bool')
                    ->set_value(true)

                    // optional property settings
                    ->set_visibility('protected')
                    ->set_static(false)
                    ->set_setter(true)
                    ->set_getter(true)
                    ->set_listProperty(true)
                    ->set_createStaticPropertyGetter(true)
                    ->set_setValueInConstructor(true)
                    ->set_explicitMethodForValue(false)
            )
    )
;

// generate
$oDTGenerator = \MVC\Generator\DataType::create()->initConfigObject($oDTConfig);
~~~

<!--
<a id="init_array"></a>
## Init with Config: array
~~~php
$oDTGenerator = \MVC\Generator\DataType::create()->initConfigArray($aConfig['MODULE_DATATYPE_CONFIG']);
~~~
-->

<a id="array_config"></a>
### Configuration as Array

_Example_  
~~~php
// config
$aConfigDataType = array(

    // where to place the DataType Class Files.
    // folder will be created if not exists.
    'dir' => $aConfig['MVC_MODULES'] . '/Foo/DataType/',

    // remove dir and create for new each time config changes.
    // you may want this during development.
    // default is `false`, if not set here
    'unlinkDir' => false,

    // The Classes
    'class' => array(

        array(
            // ! mandatory
            'name' => 'DTFoo',
            'file' => 'DTFoo.php',

            // optional; no need to even note the key here if not used
            'extends' => '',

            // optional; no need to even note the key here if not used
            'namespace' => 'Foo\DataType',

            // optional; add some useful Helper Methods like '__toString()` method (default: true)
            'createHelperMethods' => true,
            
            // optional; no need to even note the key here if not used
            'constant' => array(
                array(
                    'key' => 'FOO',
                    'value' => 'BAR',
                    'visibility' => 'public'
                )
            ),

            // ! mandatory
            'property' => array(
                array('key' => 'sKey'               , 'var' => 'string'),
                array('key' => 'deliverable'        , 'var' => 'int'),
                array('key' => 'aJsonContext'       , 'var' => 'array'),
                array('key' => 'bSuccess'           , 'var' => 'bool'),
                array(
                    'key' => 'foo',
                    'var' => 'string', 
                    
                    // optional property settings
                    'value' => 'bar',
                    'visibility' => 'protected'                    
                    'static' => false,
                    'setter' => true,
                    'getter' => true,
                    'explicitMethodForValue' => false,
                    'listProperty' => true,
                    'createStaticPropertyGetter' => true,
                    'setValueInConstructor' => true,
                ),
            )
        ),
    )
);

// generate
$oDTGenerator = \MVC\Generator\DataType::create()->initConfigArray(aConfigDataType);
~~~

<a id="Hint"></a>
## Hint
- use `bool`, not `boolean`
- use `int`, not `integer`

<a id="Valid_types"></a>
## Valid types
- https://www.php.net/manual/en/functions.arguments.php#functions.arguments.type-declaration