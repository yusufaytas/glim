### glim
Glim is the new framework for API design for python. It has no view
layers at least currently. The main idea to build a small structure of a
web app that can handle requests. It has SQLAlchemy integration that can
map models to DB dynamically. Moreover, it has integration with werkzeug
and some powerful features about routing

### Installation
Currently, only cloning this repo is required for the app. However,
virtualenv can be used for installing pip dependancies of the project.

Clone the repo
```
$ git clone git@github.com:aacanakin/glim.git
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
$ pip install -r reqs
```
Start web server
```
$ python glim.py start
```

NOTE: PyPI integration will be finished after first release for easy to
install

### Config
#### Environments
#### The config structure

### Routing
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

#### Route filtering
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

#### Route grouping
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

### Controllers
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

#### Restful controllers
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

### Models
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

### Services
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
        user = User(fullname = "Aras Can Akin", title = "glim developer")

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

### Extension System
#### Configuration
#### Integration
#### A redis extension example

### Internal Framework Components
#### Facades
#### The registry
#### Database & ORM

