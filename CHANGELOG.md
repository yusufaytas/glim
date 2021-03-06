```
---------------------- v0.7.9 06/09/2014 ----------------------
- fixed static path not registering SharedDataMiddleware
- added feature for static paths to be dynamically configured with url
- fixed a bug on app that prevents view layer from booting
- removed facades, extensions from default config
- added feature for extensions to be smartly registered from config
---------------------------------------------------------------

---------------------- v0.7.8 04/09/2014 ----------------------
- PYPI package release !!
- fixed loads of pypi bugs
- fixed loads of import bugs when loaded from pypi
- added detection of dev or pypi mode
- added paths module to find out framework, projects paths
- refactored glim.py into glim/cli.py
- moved proto into protoype
- improved import_module() in glim.utils
- seperated glim.app module into glim.app & glim.dispatch
---------------------------------------------------------------

---------------------- v0.7.7 26/08/2014 ----------------------
- refactored Facade booting into Facade registering
- seperated sessions, views from glim config
- added pass_errors variable to utils.import_module()
- removed Extension class extending Registry
- added Response to core module
- disabled comments in the proto to make app say hello
- added colorful logging with Log facade
- added logging configuration to proto/default config
- added feature for extensions to have command line utils
- bunch of fixes in app startup
- other minor fixes
---------------------------------------------------------------

---------------------- v0.7.2 17/08/2014 ----------------------
- updated gitignore not to include app related folders
- removed not implemented Schema & Migration classes
- added ascii glim!
- added the new command layer for glim!
- implemented start & new commands with the new command layer
- added feature to create custom commands with use of booted objects
- added feature to create commands with app object
- moved host,port options to App.run() functions from constructor
- fixed new command not generating a new app
- copytree(), import_module() moved to glim.utils
- removed click dependancy
- made other small changes
- rewritten app boot script
---------------------------------------------------------------

---------------------- v0.6.7 02/08/2014 ----------------------
- added feature for static files to be loaded directly by /static route
- added static folder to project prototype
- fixed a bug that prevents static folder scripts from directly loading
- minor non-functional changes
---------------------------------------------------------------

---------------------- v0.6.4 02/08/2014 ----------------------
- implemented session & cookie support
- implemented view layer with jinja2
- moved usable framework components to component module
- moved storage folder into app
- removed redis dependancy
- removed ClassNameDescriptor
- renamed registry class imports
- updated config in prototype project
- fixed a bug that prevents starting glim without python call
- removed some prints
---------------------------------------------------------------

---------------------- v0.4.5 02/08/2014 ----------------------
- added IoC facade for instance binding & resolving
- added feature for returning either Response object or basestring in controllers
- added feature for registering a callback before web server starts
- made code organization in web server bootups & start. It's more object-oriented now
- fixed a bug causing problems for filters that have string base returns
- fixed a bug causing problems for root route filters
---------------------------------------------------------------

---------------------- v0.3.3 29/07/2014 ----------------------
- introduced new command for generating a new glim app
- app related folders removed from repo
- database related files moved into glim.db module
- added project prototype structure in glim/proto/project folder
---------------------------------------------------------------

---------------------- v0.2.1 25/07/2014 ----------------------
- initial extension system is finished!
- written a sample extension namely "gredis" for test purposes
- optional bootup for extension system implemented
- unnecessary file removals
---------------------------------------------------------------

---------------------- v0.1.12 23/07/2014 ----------------------
- added storage folder for keeping logs, etc.
- added initial prototypes for generating controllers, models & services from command line
- removed cookie, session support from first stable release
- changed configuration structure (See config/default.py)
- add Orm to facades, add bootup after Db bootup
- initial extension system integrated, default config changed accordingly
- fixed an issue that prevents auto facade loading from config
- fixed an issue that prevents config loading
- fixed an issue that crashes everything
- commented out sample db config, enabled extensions
- added route filtering feature !
- added route grouping feature !
---------------------------------------------------------------

---------------------- v0.0.7 20/07/2014 ----------------------
- SQLAlchemy initial model mapping is finished, need to cleanup
- add orm session system from SQLAlchemy's sessionmaker & Session class
- fixed an indentation bug that prevents web server boot
- fixed a bug which raises a syntax error in routes file
- code cleanups, delete trailing spaces in test app
---------------------------------------------------------------

---------------------- v0.0.6 24/05/2014 ----------------------
- SQLAlchemy integration started
- DB related classes are added into core
- added mysql-python dependancy
- removed cli.py, command line tools will be implemented w/ click
- changed routes data structure
- added feature for connecting multiple databases in config
- added samples for glim app usage
- services are changed into facades
- Service layer is introduced in app context
---------------------------------------------------------------

---------------------- v0.0.2 18/05/2014 ----------------------
- config folder moved into app
- static folder definition is disabled initially
- after function is removed from start.py
---------------------------------------------------------------

---------------------- v0.0.1 13/05/2014 ----------------------
- Routing is implemented and currently working
- Implicit restful routing is implemented
- Controller dispatch_request() relation is implemented
- Werkzeug is integrated
- Response class removed from Controllers constructor
- use_reloader, use_debugger settings added in app boot
- initial dynamic service loading is implemented
---------------------------------------------------------------

---------------------- 10/05/2014 ----------------------
- Service layer fixed
- Registry class is finalized
- Dynamically static function call is implemented in Service
- boot() function is implemented
- set() function bug is fixed
- services is added in configuration
- initial wsgi app class implemented
- modular service boots initially implemented
--------------------------------------------------------

---------------------- 30/04/2014 ----------------------
- added click package dependancy
- added initial commands with use of click
- added Registry class and its extensions
- added cli module
- changed initial file/folder structure
- added migrations module
- added commands module
- added routes module
- added colorful commands
- added services module
--------------------------------------------------------
```
