# Installation

make sure that your local machine has PHP and Composer installed. 
After you have installed PHP and Composer, you may create a new myMVC project via the Composer create-project command:

_get latest sources from main_  
~~~bash
composer create-project gueff/myMVC myMVC
~~~

<!--
_install latest version 1.3.x_  
~~~bash
composer create-project gueff/myMVC myMVC 1.3
~~~
-->

cd into the root folder of myMVC, where the file myMVC.phar resides.

<!--
~~~bash
cd myMVC/public; ./serve.sh
~~~
-->

_run `myMVC.phar`_  
~~~bash
cd myMVC;
php myMVC.phar
~~~

The Auto-Installer will instantly begin to install all necessary files. (In case of errors, a text will prompt up showing details about what went wrong). This may take a moment.

~~~bash
setup checking
• MVC_ENV is: develop
• User/Group from /public/index.php: admin1(1000) / admin1(1000)
• Installing required Main Application libraries via composer in Background with PID 84623. Please wait.
................Installation completed.
~~~

**Run myMVC** <a name="Run_myMVC"></a>    

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
~~~

Now you can call your application in your web browser at [http://127.0.0.1:1969](http://127.0.0.1:1969).

<a href="https://mymvc.ueffing.net/@image/getting-started/mymvc-installation/png/@size/" data-lightbox="gallery"><img class="figure-img img-fluid" src="https://mymvc.ueffing.net/@image/getting-started/mymvc-installation/png/@size/" title="mymvc-creating-a-module"></a><div style="clear:both;"></div>
