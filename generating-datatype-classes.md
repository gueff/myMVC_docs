
# Generating DataType Classes

myMVC includes a Generator that you can use to generate DataType Classes. Consider those Classes more than a Storage than a Logic Element.  
Define what name the class and which namespace it should have, which properties or constants it should provide.  
Then just run the Generator and it will create the Class for you.

- [Configuration](#Configuration)
  - [Array Notation](#array_config)
  - [Object Notation](#object_config)
- [Note regarding php data types](#Hint)

---

<a id="Configuration"></a>
## Configuration

Write your own Configurations.

_Place for DataType Generating Configurations; (assuming module `Foo`)_  
~~~
/modules/Foo/etc/config/DataType/
~~~

There is already a Configuration file `datatype.php`

~~~
/modules/Foo/etc/config/DataType/datatype.php
~~~

Best way to start is to extend this file.  
After you made your edits, just run the file on command line

~~~bash
cd /modules/Foo/etc/config/DataType;
php myDataTypeClass.php
~~~

<a id="array_config"></a>
### Configuration as Array

_Example_  
~~~php
$aDataType['class'][] = array(
    // ! mandatory
    'name' => 'DTFoo',
    'file' => 'DTFoo.php',

    // optional; no need to even note the key here if not used
    'extends' => '',

    // optional; no need to even note the key here if not used
    'namespace' => \MVC\Config::get_MVC_MODULE_CURRENT_NAME() . '\DataType',

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
            'forceCasting' => true,
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
);
~~~

<a id="object_config"></a>
### Configuration as Object

You can also create a DataType class the object way. 

_Example_
~~~php
// config
$oDTConfig = \MVC\DataType\DTConfig::create()
    ->set_dir(\MVC\Config::get_MVC_MODULES_DIR() . '/' . \MVC\Config::get_MVC_MODULE_CURRENT_NAME() . '/DataType/')
    ->set_unlinkDir(false)
    ->add_DTClass(

        \MVC\DataType\DTClass::create()
            ->set_name('DTFoo')
            ->set_file('DTFoo.php')
            ->set_namespace(\MVC\Config::get_MVC_MODULE_CURRENT_NAME() . '\DataType')
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
                    ->set_forceCasting(true)
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

---

<a id="Hint"></a>
## Note regarding php data types

- use `bool`, not ~~`boolean`~~
- use `int`, not ~~`integer`~~
