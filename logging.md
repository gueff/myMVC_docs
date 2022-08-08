
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
- IP Address
- a uniqueID for the current Request
- the Session ID
- an increasing Counter for each log entry for the time Request is running
- the file and lineNr from where the log was called
- The Log Message

Extra LogInfos for Events  
- `BIND` with the Eventname and where the Event was called from.
- `RUN`	with the Eventname and where the Event was called from. No further logic is bonded to that Event actually
- `RUN+` with the Eventname and where the Event was called from. In this case some logic was bonded to that event (via "BIND") and all bonded logics are listed in detail