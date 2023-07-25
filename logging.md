
# Logging

- [Configuration](#configuration)

------------------------------------------------------------------------------------------------------------------------

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
2023-07-25 16:08:43     foo.local       develop 127.0.0.1       2023072516084364bfd76bbbfa8     ...........no.session...........        1       /application/init/util/bootstrap.php, 111       ##########      new Request     apache2handler  GET /
~~~

---

<a id="configuration"></a>
## Configuration

Preferably change the settings in the [environment configuration file](/3.3.x/configuration#Modules-environment-config-file) of your module according to your needs.

_Settings for Logging_  
~~~php
/**
 * Log
 */
$aConfig['MVC_LOG_FILE_FOLDER'] = $aConfig['MVC_APPLICATION_PATH'] . '/log/';
$aConfig['MVC_LOG_FILE_DEFAULT'] = $aConfig['MVC_LOG_FILE_FOLDER'] . 'default.log';
$aConfig['MVC_LOG_FILE_ERROR'] = $aConfig['MVC_LOG_FILE_FOLDER'] . 'error.log';
$aConfig['MVC_LOG_FILE_WARNING'] = $aConfig['MVC_LOG_FILE_FOLDER'] . 'warning.log';
$aConfig['MVC_LOG_FILE_NOTICE'] = $aConfig['MVC_LOG_FILE_FOLDER'] . 'notice.log';
$aConfig['MVC_LOG_FILE_POLICY'] = $aConfig['MVC_LOG_FILE_FOLDER'] . 'policy.log';
$aConfig['MVC_LOG_FILE_EVENT'] = $aConfig['MVC_LOG_FILE_FOLDER'] . 'event.log';

// control log details
$aConfig['MVC_LOG_DETAIL'] = [
    'date' => true,
    'host' => true,
    'env' => true,
    'ip' => true,
    'uniqueid' => true,
    'sessionid' => true,
    'count' => true,
    'debug' => true,
    'message' => true,
];
~~~