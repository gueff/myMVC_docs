<!--[Getting Started]-->
# Creating a Module

- cd into the root folder of myMVC, where the file `myMVC.phar` resides.
- run `myMVC.phar`, it will show you a menu with some options.
- enter `1` to create a new module.

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
myMVC:~$  [<enter> = 0] 1
~~~

_Enter a modulename - in this example it is `Foo`_  
~~~bash
––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
myMVC
a php framework by G. Üffing
––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––––
Enter Name of the new Module:  Foo

you entered: `Foo`
should the module `Foo` be created now ? [<enter> = y]

you entered: `y`

...creating module/Foo/* with subdirectories and -files
 ✔ module 'Foo' created.

press enter key to continue
unMVC:~$  
~~~

Call http://127.0.0.1:1969/ and you will see your new created module frontend

<a href="https://mymvc.ueffing.net/@image/getting-started/mymvc-creating-a-module/png/@size/" data-lightbox="gallery"><img class="figure-img img-fluid" src="https://mymvc.ueffing.net/@image/getting-started/mymvc-creating-a-module/png/@size/600/" title="mymvc-creating-a-module"></a><div style="clear:both;"></div>

_directory structure of the created module `Foo`_  
~~~bash
.
├── Controller
│   └── Index.php
├── DataType
├── etc
│   ├── config
│   │   ├── Cachix.php
│   │   ├── DataType
│   │   │   └── MODULE_DATATYPE.php
│   │   ├── Foo
│   │   │   ├── composer.json
│   │   │   └── config
│   │   │       └── develop.php
│   │   ├── _init.php
│   │   ├── _MVC.php
│   │   └── policy.php
│   ├── doc
│   ├── _INSTALL
│   │   └── public
│   │       └── robots.txt
│   └── routing
│       ├── _createFinalJson.php
│       ├── final.json
│       └── frontend.json
├── Event
│   └── Index.php
├── generateDataTypes.php
├── install.sh
├── Model
│   └── Index.php
├── Policy
│   └── Index.php
├── publish.sh
├── templates
│   └── Frontend
│       ├── content
│       │   ├── 404.tpl
│       │   ├── _cookieConsent.tpl
│       │   ├── index.tpl
│       │   └── _noscript.tpl
│       └── layout
│           ├── footer.tpl
│           ├── header.tpl
│           ├── index.tpl
│           └── menu.tpl
└── View
    └── Index.php
~~~
