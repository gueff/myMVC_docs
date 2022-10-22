
# Logging

_default path for logfiles_  
~~~
/application/log/
~~~

_write into default logfile_ 
~~~php
\MVC\Log::write('My Message');
~~~
- writes to: `/application/log/default.log`

_write into another logfile_  
~~~php
\MVC\Log::write('My Message', 'specialLogfile.log');
~~~

All Log Entries show:  
- Date and Time
- Host
- Environment
- IP Address
- A uniqueID for the current Request
- the Session ID (if any)
- An increasing Counter for each log entry for the time Request is running
- The file and lineNr from where the log was called
- The Log Message

Extra LogInfos for Events  
- `BIND` with the Eventname and where the Event was called from.
- `RUN`	with the Eventname and where the Event was called from. No further logic is bonded to that Event actually
- `RUN+` with the Eventname and where the Event was called from. In this case some logic was bonded to that event (via "BIND") and all bonded logics are listed in detail

_Example_  
~~~
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   ...........no session...........        0       /application/config/util/bootstrap.php, 123 >   ##########      new Request     apache2handler  GET /
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   ...........no session...........        1       /application/config/util/bootstrap.php, 221 >   AUTOLOADING     MVC/MVCTrait/TraitDataType.php
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   ...........no session...........        2       /application/library/MVC/Event.php, 54  BIND (mvc.error, function (oDTArrayObject $oDTArrayObject){
                        \MVC\Error::addERROR ($oDTArrayObject);
                });
) --> called in: /application/library/MVC/Error.php, 61
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   ...........no session...........        3       /application/config/util/bootstrap.php, 221 >   AUTOLOADING     Foo/DataType/DTRoutingAdditional.php
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   ...........no session...........        4       /application/config/util/bootstrap.php, 221 >   AUTOLOADING     Foo/Controller/Index.php
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   ...........no session...........        5       /application/config/util/bootstrap.php, 221 >   AUTOLOADING     Foo/Event/Index.php
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   ...........no session...........        6       /application/library/MVC/Event.php, 54  BIND (mvc.application.setSession.before, function (oDTArrayObject $oDTArrayObject){
            \Foo\Event\Index::enableSession($oDTArrayObject);
        }
	) --> called in: /modules/Foo/Event/Index.php, 48
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   ...........no session...........        7       /application/library/MVC/Event.php, 54  BIND (mvc.controller.init.before, function (oDTArrayObject $oDTArrayObject){
            \Idolon\Event\Index::startIdolon($oDTArrayObject);
        }
) --> called in: /modules/Foo/Event/Index.php, 48
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   ...........no session...........        8       /application/library/MVC/Event.php, 54  BIND (mvc.reflex.reflect.targetObject.before, function (oDTArrayObject $oDTArrayObject){
//            \MVC\Debug::debug($oDTArrayObject);
            \MVC\Minify::init();
        }
) --> called in: /modules/Foo/Event/Index.php, 48
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   ...........no session...........        9       /application/library/MVC/Event.php, 54  BIND (mvc.reflex.reflect.targetObject.after, function (oDTArrayObject $oDTArrayObject){
            if (true === Config::get_MVC_INFOTOOL_ENABLE())
            {
                // switch on InfoTool
                new \MVC\InfoTool(\Foo\View\Index::init());
            }
        },
) --> called in: /modules/Foo/Event/Index.php, 48
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   ...........no session...........        10      /application/library/MVC/Event.php, 54  BIND (mvc.debug.stop.after, function (oDTArrayObject $oDTArrayObject){
            \MVC\Log::write("\n\n*** STOP ***\n" . print_r($oDTArrayObject->get_akeyvalue()[0]->get_sValue(), true));
        }
) --> called in: /modules/Foo/Event/Index.php, 48
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   ...........no session...........        11      /application/library/MVC/Event.php, 110 RUN+ (mvc.application.setSession.before) --> called in: /application/library/MVC/Application.php, 71 --> bonded by `/modules/Foo/Event/Index.php, 48, try to run its Closure: function (oDTArrayObject $oDTArrayObject){
            \Foo\Event\Index::enableSession($oDTArrayObject);
        }

2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   2e68203502766860bd3a982f97a5cebe        12      /application/library/MVC/Event.php, 110 RUN+ (mvc.controller.init.before) --> called in: /application/library/MVC/Controller.php, 28 --> bonded by `/modules/Foo/Event/Index.php, 48, try to run its Closure: function (oDTArrayObject $oDTArrayObject){
            \Idolon\Event\Index::startIdolon($oDTArrayObject);
        }

2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   2e68203502766860bd3a982f97a5cebe        13      /application/config/util/bootstrap.php, 221 >   AUTOLOADING     Idolon/Event/Index.php
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   2e68203502766860bd3a982f97a5cebe        14      /application/config/util/bootstrap.php, 221 >   AUTOLOADING     Foo/View/Index.php
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   2e68203502766860bd3a982f97a5cebe        15      /application/library/MVC/Event.php, 54  BIND (mvc.view.render.off, function (){
            \MVC\View::$bRender = false;
        });
) --> called in: /application/library/MVC/View.php, 96
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   2e68203502766860bd3a982f97a5cebe        16      /application/library/MVC/Event.php, 54  BIND (mvc.view.render.on, function (){
            \MVC\View::$bRender = true;
        });
) --> called in: /application/library/MVC/View.php, 100
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   2e68203502766860bd3a982f97a5cebe        17      /application/library/MVC/Event.php, 54  BIND (mvc.view.echoOut.off, function (){
            \MVC\View::$bEchoOut = false;
        });
) --> called in: /application/library/MVC/View.php, 104
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   2e68203502766860bd3a982f97a5cebe        18      /application/library/MVC/Event.php, 54  BIND (mvc.view.echoOut.on, function (){
            \MVC\View::$bEchoOut = true;
        });
) --> called in: /application/library/MVC/View.php, 108
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   2e68203502766860bd3a982f97a5cebe        19      /application/library/MVC/Event.php, 110 RUN+ (mvc.reflex.reflect.targetObject.before) --> called in: /application/library/MVC/Reflex.php, 131 --> bonded by `/modules/Foo/Event/Index.php, 48, try to run its Closure: function (oDTArrayObject $oDTArrayObject){
//            \MVC\Debug::debug($oDTArrayObject);
            \MVC\Minify::init();
        }

2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   2e68203502766860bd3a982f97a5cebe        20      /application/library/MVC/Event.php, 110 RUN+ (mvc.reflex.reflect.targetObject.after) --> called in: /application/library/MVC/Reflex.php, 165 --> bonded by `/modules/Foo/Event/Index.php, 48, try to run its Closure: function (oDTArrayObject $oDTArrayObject){
            if (true === Config::get_MVC_INFOTOOL_ENABLE())
            {
                // switch on InfoTool
                new \MVC\InfoTool(\Foo\View\Index::init());
            }
        },

2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   2e68203502766860bd3a982f97a5cebe        21      /application/library/MVC/Event.php, 54  BIND (mvc.view.render.before, function (oDTArrayObject $oDTArrayObject){
            InfoTool::injectToolbar ($oDTArrayObject->getDTKeyValueByKey('oView')->get_sValue());
        });
) --> called in: /application/library/MVC/InfoTool.php, 36
2022-10-04 08:50:02     mymvc.ueffing.local     develop 127.0.0.1       633bd79a39b6e   2e68203502766860bd3a982f97a5cebe        22      /application/library/MVC/Event.php, 110 RUN+ (mvc.view.render.before) --> called in: /application/library/MVC/View.php, 189 --> bonded by `/application/library/MVC/InfoTool.php, 36, try to run its Closure: function (oDTArrayObject $oDTArrayObject){
            InfoTool::injectToolbar ($oDTArrayObject->getDTKeyValueByKey('oView')->get_sValue());
        });
~~~
