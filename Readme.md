## Getting Started
Glim is the new framework for API design for python. It has no view
layers at least currently. The main idea to build a small structure of a
web app that can handle requests. It has SQLAlchemy integration that can
map models to DB dynamically. Moreover, it has integration with werkzeug
and some powerful features about routing

### Installation & Quickstart
Currently, only cloning this repo is required for the app. However,
virtualenv can be used for installing pip dependancies of the project.

Create a dir
```
$ mkdir <project-name>
```

Clone the repo
```
$ git clone git@github.com:aacanakin/glim.git <project-name>
```

Install pip if not installed
```
$ sudo easy_install pip
```

Install virtualenv
```
$ sudo easy_install virtualenv
OR
$ sudo pip install virtualenv
```

Create & activate virtualenv inside `glim` folder
```
$ virtualenv venv
$ . venv/bin/activate
```

Install dependencies
```
$ pip install glim
```

Generate a new app
```
$ glim new
```

Copy an alias of default configuration namely the environment
```
$ cp app/config/default.py app/config/development.py
```

Start the web server
```
$ glim start
```

## App structure
In glim, the `new` command generates an app folder with the following folder structure;
```
app
├── commands.py    => custom command line utilities
├── config         => configuration sources
│   └── default.py => a sample default configuration
├── controllers.py => controller sources
├── models.py      => model layer sources
├── routes.py      => application routes source
├── services.py    => application services source
├── static         => static folder to hold css,img,js,etc.
├── start.py       => functions run before the web app starts
└── storage        => storage folder for logs, sessions, etc.

ext                => ext folder to keep extensions
```

## Configuration
Glim has configuration module for application wide constants. The default configuration resides in `app/config/default.py`. The config structure is the following;

```python
# config.py
import os
import glim.paths

config = {

    'extensions' : {
        # 'gredis' : {
        #   'default' : {
        #       'host' : 'localhost',
        #       'port' : '6379',
        #       'db'   : 0
        #   }
        # }
    },

    # database configuration
    'db' : {
        # 'default' : {
        #   'driver' : 'mysql',
        #   'host' : 'localhost',
        #   'schema' : 'test',
        #   'user' : 'root',
        #   'password' : '',
        # },
    },

    'orm' : True,
    
    'log' : {
        # 'level' : 'info',
        # 'format' : '[%(levelname)s] : %(message)s',
        # 'file' : 'app/storage/logs/debug.log'
    },

    'views' : {
        # package to be loaded by jinja2
        'package' : 'app.views'
    },

    'sessions' : {
        # session id prefix
        'id_header' : 'glim_session',
        'path' : glim.paths.STORAGE_PATH,
    },
    
    'app' : {
        'reloader' : True,
        'debugger' : True,
        'static' : {
            'path' : glim.paths.STATIC_PATH,
            'url'  : '/static'
        }
    }
}
```

In `config` dict, there are configurations for extensions, db and glim.

It can be created custom configuration variables. These variables can be accessible with `Config.get()`. It can be deeply accessed by the following;

```python
Config.get('db.default.user')
# returns 'root'

Config.set('db.default.password', 'glim-rocks')
# sets config['db']['default']['password'] as 'glim-rocks'
```

Framework wide configuration should reside in `glim` dict.

### Environments
In glim, the default environment can't be used because it is a **sample** of configuration. To create a development environment, type the following;

```
$ cp app/config/default.py app/config/development.py
```

Now, we have a development environment. Glim can start a web server with a defined environment by following;
```
$ glim start --host 127.0.0.1 --port 8080 --env development
```

This command loads configuration of `app/config/development.py` config.

## Routing
Glim has powerful routing feature which developers can define routes
which will be mapped to controllers. The route definition file is at
`app/routes.py` The simplest route definition would be the following;
```python
# routes.py
urls = {
    '/' : 'BaseController.hello'
    '/restful' : 'BaseController'
}
```

This definition simply means that the root route '/' will be mapped into
`BaseController`'s `hello()` function.

As you might notice, there exists '/restful' route which maps
'BaseController' request methods. That means a `POST /restful` will be
mapped into
`BaseController.post()`. The supported requests methods are `get, post,
put, patch, delete`.

### Filtering
One of the great feature of routing system is filtering. You can define
filters which will be run sequential. This feature is mostly used in
input validation. The simplest route filtering would be the following;
```python
# routes.py
urls = {
    '/' : [
        'BaseController.validate',
        'BaseController.hello'
    ]
}
```

This definition simply means that the root route '/' is mapped to
`BaseController`'s `validate()` & `hello()` function sequentially.

**Important Note**: If `validate()` function returns an instance of
`Response`, then `hello()` function won't be called

### Grouping
This is also one of the greatest feature of glim. People always want to
create grouped routes. The simplest route grouping would be the
following;
```python
# routes.py
urls = {
    '/api' : {
        '/auth' : 'ApiController.auth',
        '/me' : [
            'ApiController.check_auth',
            'ApiController.me'
        ]
    }
}
```
Here, we have defined two routes; `/api/auth` & `/api/me`. The `/api/me`
function has a filter namely `check_auth` as you noticed.

**Important Note**: In route grouping, you need to define filters for
each endpoint repeatedly for not having ambiguous definition for routes.

## Request & Response

## Views

## Controllers
The controllers are no different than any other MVC framework's
controllers. The controllers file resides in `app/controllers.py`. The
simplest controller definition would be the following;
```python
# controllers.py
from glim.core import Controller
from werkzeug.wrappers import Response

class BaseController(Controller):
    def hello():
        return Response('Hello World Mate!')
```
This controller definition could be mapped to routes as
`BaseController.hello`
To see this text on browser, `Response` object should be returned

### Restful controllers
To make controllers restful, it is highly recommended to extend
`RestfulController` class. The simplest restful controller would be the
following;
```python
# controllers.py
from glim.core import RestController
from werkzeug.wrappers import Response

class RestfulController(RestController):

    def get(self):
        return Response('RESTful GET')

    def post(self):
        return Response('RESTful POST')

    def put(self):
        return Response('RESTful PUT')
```
**Important Note**: A restful controller can not be used as a route
filter

## Models
This layer is used for SQLAlchemy model integration to glim. In glim,
models are mapped to database tables dynamically w/ SQLAlchemy. The
models file resides in `app/models.py`. The simplest model definition
would be the following;
```python
# models.py
from glim.core import Model
from sqlalchemy import Column, Integer, String

class User(Model):

    __tablename__ = 'users'
    id = Column(Integer, primary_key = True)
    fullname = Column(String(255))
    title = Column(String(255))

    def __repr__(self):
        return "<User (name = %s,title = %s)" % (self.full_name,
self.title)
```
This is not any different than it is documented on SQLAlchemy's
documentation website.

## Services
Services is not a must layer but it is very useful for seperating
database models with model or db operations. The services file resides
in `app/services.py`. Assume that we have a model namely `User` above,
the simplest service definition would be the following;
```python
# services.py
from glim.core import Service
from glim.facades import Database as DB, Orm as ORM
from models import User

class UserService(Service):
    @staticmethod
    def register(full_name = 'Aras Can', title = 'glim developer'):
        # DB.execute("INSERT INTO users (full_name, title) VALUES ('%s',
        # '%s')" % (full_name, title))

        # generate an instance of User
        user = User(full_name, title)

        # writes a new user to the database session
        ORM.add(user)

        # commits as SQLAlchemy suggests
        ORM.commit()
```
From my perspective, Services should be stateless and therefore
`static`. However, it's not a must to use them static. You can
instantiate in controllers to use them.

In controllers, `UserService.register(full_name, title)` would register
a user in the database.

## Extension System
Glim has an extension system that enables developers to deeply integrate modules into glim. In `glim.core` module there exists two classes for extension development namely `Extension` and `Facade`. In glim, facades are used to register an instance of object statically with respect to config for usage. Therefore, **if you want an extension to be loaded up during web server start**, you need to extend facade.

### Configuration
Currently, it is needed to write config of extension in main config file in extensions. Assume that we have a redis extension namely `"gredis"`. The config inside extensions would be the following;

```python
#config.py
config = {
    'extensions' = {
        'gredis' : {
            'default' : {
                'host' : 'localhost',
                'port' : 1234,
                'db' : 0
            }
        }
    },
    # ...
}
```

An example of a redis extension would be the following;

```python
# ext/gredis.py
from glim.core import Facade
from glim.component import Extension
from glim.facades import Log

import redis

class GredisExtension(Extension):
    def __init__(self, config):
        self.config = config
        self.active = 'default'
        self.connections = {}

        for k, config in self.config.items():
            self.connections[k] = self.connect(config)

    def __getattr__(self, attr):
        try:
            return getattr(self.connections[self.active], attr)
        except redis.RedisError, e:
            Log.error(e)
            return None

    def connection(self, key = None):
        if key:
            self.active = key
        else:
            self.active = 'default'

    def connect(self, config):
        try:
            connection = redis.Redis(
                host = config['host'],
                port = config['port'],
                db = config['db'])

            connection.ping()
            return connection

        except redis.RedisError, e:
            Log.error(e)
            return None

class Gredis(Facade):
    pass
```

When you start the web server, `GredisExtension` class would be instantiated with config inside config.py and put inside `Gredis` class which is a `Facade`. It is accessible `Gredis.get('foo', 'bar')`.

Extending `Facade` is completely optional but highly recommended. If `Facade` extension is not used, then configuration is not needed.

The name convention is the following;
- The class to be instantiated and registered should have name with `<module>Extension` and the facade name should be same as module name if facades are used. Otherwise, there aren't any conventions

## Database & ORM
## Facades
## The Registry
