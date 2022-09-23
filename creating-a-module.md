<!--[:Getting Started]-->

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

Now [🏁 run local development server](/3.x/installation#Run_myMVC) again.  
Call http://127.0.0.1:1969/ and you will see your new created module frontend

![myMVC Creating a Module](/doc/getting-started/mymvc-creating-a-module.png)


_directory structure of the created module `Foo`_  
- see: [Directory Structure](/3.x/directory-structure)