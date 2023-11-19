
# Directory Structure

- [`/`](#root)
- [`/application/`](#application)
- [`/modules/{moduleName}/`](#modules-moduleName)
- [`/modules/{moduleName}/etc/`](#modules-moduleName-etc)
- [`/modules/{moduleName}/etc/config/`](#modules-moduleName-etc-config)
- [`/modules/{moduleName}/etc/config/{moduleName}/config/`](#modules-moduleName-etc-config-moduleName-config)
- [`/modules/{moduleName}/templates/`](#modules-moduleName-templates)

---

<a id="root"></a>
## `/`

| Folder / File                                         | Meaning                                            |
|-------------------------------------------------------|----------------------------------------------------|
| 📁 [application](#application)                        | myMVC Framework and libraries, temporary files     |    
| 📁 [config](/3.4.x/configuration#myMVC-config-folder) | top config folder; gobal                           |  
| 📁 [modules](#modules-moduleName)                     | **&larr; in here you write your application code** |    
| 📁 public                                             | any public files like `*.css`, `*.js`              | 
| emvicy.php                                            | command line tool; helps to manage                 |  

---

<a id="application"></a>
## `/application/` 

| Folder / File    | Meaning                                     |
|------------------|---------------------------------------------|
| 📁 cache         | place for caching files                     |
| 📁 init          | skeleton files and utilities                |
| 📁 library       | Core Framework                              |  
| 📁 log           | default logfile directory                   |
| 📁 session       | SessionIDs are stored here by default       |
| 📁 smartyPlugins | default smartyPlugin directory              |  
| 📁 templates_c   | default home for compiled smarty templates  |  
| 📁 vendor        | third party libraries installed by composer |
| composer.json    | list of third party libraries to install    |  
| composer.phar    | a standalone composer script                |  


<a id="modules-moduleName"></a>
## `/modules/{moduleName}/` 

| Folder / File                                 | Meaning                                                                                             |
|-----------------------------------------------|-----------------------------------------------------------------------------------------------------|
| 📁 Controller                                 | Your Application Controller Classes                                                                 |
| 📁 DataType                                   | Your generated DataType Classes                                                                     |
| 📁 [etc](#modules-moduleName-etc)             | place for install- and config files, docs, routing and individual other stuff                       |
| 📁 Model                                      | Your Application Model Classes                                                                      |
| 📁 Policy                                     | Your Application Policy Classes                                                                     |
| 📁 [templates](#modules-moduleName-templates) | Template files                                                                                      |
| 📁 View                                       | Your Application View Classes                                                                       |
| _install.sh                                   | helper bash script to e.g. install files from `modules/{moduleName}/etc/_INSTALL/` to other places  |
| _publish.sh                                   | helper bash script to copy files from `modules/{moduleName}/etc/_INSTALL/public/` to `public/`      |


<a id="modules-moduleName-etc"></a>
## `/modules/{moduleName}/etc/`

| Folder / File                               | Meaning                                                                                                 |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------|
| 📁 _INSTALL                                 | place for files to install _(e.g. copy into `public` folder)_                                           |
| 📁 [config](#modules-moduleName-etc-config) | Module's config files                                                                                   |
| 📁 doc                                      | place for any further Module documentation                                                              |
| 📁 event                                    | place for Event Listeners. See [Registering Event Listeners](/3.4.x/events#registering-event-listeners) |
| 📁 policy                                   | Policy Rules                                                                                            |
| 📁 routing                                  | Routing files                                                                                           |
| 📁 smartyPlugins                            | Smarty template PlugIn files                                                                            |


<a id="modules-moduleName-etc-config"></a>
## `/modules/{moduleName}/etc/config/`

| Folder / File                                                     | Meaning                                                                  |
|-------------------------------------------------------------------|--------------------------------------------------------------------------|
| 📁 DataType                                                       | Module's DataType configuration files                                    |
| 📁 {moduleName}                                                   |                                                                          |
| └── 📁 [config](#modules-moduleName-etc-config-moduleName-config) | place for any further Module documentation                               |
| _mvc.php                                                          | `primary` Module's config - a `secondary` module does not have this file |


<a id="modules-moduleName-etc-config-moduleName-config"></a>
## `/modules/{moduleName}/etc/config/{moduleName}/config/`

| Folder / File | Meaning                                                                                                                                                        |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| _csp.php      | Content-Security-Policy rules                                                                                                                                  |
| _session.php  | Where to enable & disable Session                                                                                                                              |
| develop.php   | Module's environment config file. See [Example `/modules/Foo/etc/config/Foo/config/develop.php`](/3.4.x/configuration#Modules-environment-config-file-example) |


<a id="modules-moduleName-templates"></a>
## `/modules/{moduleName}/templates/` 

_templates directory structure_  
~~~
modules/{moduleName}/
├── templates/
│   └── Frontend/
│       ├── content/
│       │   ├── _cookieConsent.tpl
│       │   ├── _noscript.tpl
│       │   ├── 404.tpl
│       │   ├── index.tpl
│       │   └── info.tpl
│       └── layout/
│           ├── footer.tpl
│           ├── index.tpl
│           └── menu.tpl
~~~
- You may find further Information in Topic [Frontend](/3.4.x/frontend)