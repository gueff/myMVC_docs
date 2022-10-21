<!--[:Getting Started]-->

# Installation

- [Get myMVC](#Get-myMVC)
- [Requirements](#Requirements)

---

<a id="Get-myMVC"></a>
## Get myMVC

make sure that your local machine has PHP >=7.4 and Composer2 installed. After you have installed PHP and Composer, you can create a new myMVC project via the Composer create-project command  

_get myMVC via composer_  
~~~bash
composer create-project --no-install gueff/myMVC myMVC 3.1.2
~~~

cd into the root folder of myMVC, where the file myMVC.phar resides.

_run `myMVC.phar`_  
~~~bash
cd myMVC; php myMVC.phar
~~~

The Auto-Installer will instantly begin to install all necessary files. (In case of errors, a text will prompt up showing details about what went wrong). This may take a moment.

~~~bash
setup checking
• MVC_ENV is: develop
• User/Group from /public/index.php: admin1(1000) / admin1(1000)
• Installing required Main Application libraries via composer in Background with PID 84623. Please wait.
................Installation completed.
~~~

<a id="Run_myMVC"></a>
**Run myMVC**    

Now start myMVC's local development server.  
- run myMVC.phar, it will show you a menu with some options.
- enter 0 (or just press the `<enter>`-key) to run local development server.

_run `myMVC.phar`_    
~~~bash
php myMVC.phar
~~~

_menu_  
~~~bash
––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
myMVC
a php framework by G. Üffing
––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
0 = 🏁 run local development server
1 = 📦 create a module
8 = 🔍 check on errors (php lint recursively)
9 = ⛔ exit
myMVC:~$  [<enter> = 0]
you entered: 0
[Wed Jul 27 07:20:00 2022] PHP 7.4.3 Development Server (http://127.0.0.1:1969) started
~~~

Now you can call your application in your web browser at [http://127.0.0.1:1969](http://127.0.0.1:1969).

![myMVC Installation](/doc/getting-started/mymvc-installation.png)

---

<a id="Requirements"></a>
## Requirements

### PHP

You need to have the PHP Version >=7.4 installed. Also you need some PHP-Extensions installed and PHP-functions enabled as listed below.

_PHP Version_  
~~~
>=7.4
~~~

_Required PHP Extensions_  
~~~
Core
ctype
curl
date
dom
fileinfo
filter
iconv
json
mbstring
Phar
posix
Reflection
session
SimpleXML
standard
SPL
zip
~~~

_Required PHP Functions_  
~~~
mb_strlen
iconv
utf8_decode
~~~

### Composer

You need to have the composer dependency manager installed.  
Visit the project Website of composer for more details about how to set up composer.

- https://getcomposer.org/