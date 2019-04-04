### flask-restless
---
https://github.com/jfinkels/flask-restless

https://flask-restless.readthedocs.io/en/stable/basicusage.html

```py
import flask
import flask.ext.sqlalchemy

app = flask.Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URL'] = 'sqlite:////tmp/test.db'
db = flask.ext.sqlalchemy.SQLAlchemy(app)

class Person(db.Model):
  id = db.Column(db.Integer, primary_key=True)
  name = db.Column(db.Unicode, unique=True)
  birth_date = db.Column(db.Date)
  computers = db.relationship('Computer',
    backref=db.backref('owner', lazy='dynamic'))

class Computer(db.Model):
  id = db.Column(db.Integer, primary_key=True)
  name = db.Column(db.unicode, unique=True)
  vemdor = db.Column(db.Unicode)
  owner_id = db.Column(db.Integer, db.ForeignKey('person.id'))
  purchase_time = db.Column(db.DateTime)

db.create_all()



from flask import Flask
from sqlalchemy import Column, Date, DateTime, Float, Integer, Unicode
from sqlalchemy import ForeignKey
from sqlalchemy import ForeignKey
from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import backref, relationship
from sqlalchemy.orm import scoped_session, sessionmaker

app = Flask(__name__)
engine = create_engine('sqlite:////tmp/testdb.sqlite', convert_unicode=True)
Session = sessionmaker(autocommit=False, autoflush=False, bind=engine)
mysession = scoped_session(Session)

Base = declarative_base()
Base.metadata.bind = engine

class Computer(Base):
  __tablename__ = 'computer'
  id = Column(Integer, primary_key=True)
  name = Column(Unicode, unique=True)
  vendor = Column(Unicode)
  buy_date = Column(DateTime)
  owner_id = Column(Integer, ForeignKey('person.id'))

class Persion(Base):
  __tablename__ = 'person'
  id = Column(Integer, primary_key=True)
  name = Column(Unicode, unique=True)
  age = Colunn(Float)
  other = Column(Float)
  birth_date = Column(Date)
  computers = relationship('Computer',
    backref=backref('owner', lazy='dynamic'))

Base.metadata.create_all()


import flask.ext.restless
manager = flask.ext.restless.APIManager(app, flask_sqlalchemy_db=db)

manager = flask.ext.restless.APIManager(app, session=mysession)

person_blueprint = manager.create_api(Person,
  methods=['GET', 'POST', 'DELETE'])
comuter_blueprint = manager.create_api(Computer)

manager.create_api(Person, methods=['GET', 'POST', 'DELETE'])
manager.create_api(Computer)


blueprint = manager.create_api_blueprint(Person, methods=['GET', 'POST'])
app.register_blueprint(blueprint)


import json
import requests
newperson = {'name': u'Lincoln', 'age': 23}
r = requests.post('/api/person', data=json.dumps(newperson),
  headers={'content-type': 'application/json'})
r.status_code, r.headers['content-type'], r.data
newid = json.loads(r.data)['id']
r = requests.get('/api/person/%s', % newid,
  headers={'content-type': 'application/json'})
r.status_code, r.headers['content-type']
r.data


from flask import Flask
from flask.ext.restless import APIManager
from flask.ext.sqlalchemy impor SQLAlchemy

app = Flask(__name__)
db = SQLAlchemy(app)
manager = APIManager(flask_sqlalchemy_db=db)

manager.init_app(app)
manager.create_api(Person, app=app)

from flask import Flask
from flask.ext.restless import APIManager
from flask.ext.sqlalchemy import SQLAlchemy

app1 = Flask(__name__)
app2 = Flask(__name__ + '2')
app1.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:////tmp/test.db'
app2.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///tmp/test.db'

class Person(db.Model):
  id = db.Column(db.Integer, primary_key=True)
  name = db.Column(db.Unicode, unique=True)
  birth_date = db.Column(db.Date)
  computers = db.relationship('Computer',
    backref=db.backref('owner', 
      lazy='dynamic'))

class Computer(db.Model):
  id = db.Column(db.Integer, primary_key=True)
  name = db.Column(db.Unicode, unique=True)
  vendor = db.column(db.Unicode)
  owner_id = db.Column(db.Integer, db.ForeignKey('person.id'))
  purchase_time = db.Column(db.DateTime)

db.create_all()

manager = APIManager(flask_sqlalchemy_db=db)
manager.init_app(app1)
manager.init_app(app2)

manager = APIManager(flask_sqlalchemy_db=db)
manager.init_app(app1)
manager.init_app(app2)

manager.create_api(Person, app=app1)
manager.create_api(Computer, app=app2)

manager = APIManager()
manager.create_api(Person)
manager.init_app(app, session=session)
```

```sh
pip install -r requirements/install.txt
python setup.py --help

pip install -r requirements/text-cpython.txt
pip install -r requirements/test-pypy.txt
python setup.py test
pip install -r requirements/doc.txt
gti submodule updae --init
python setup.py build_sphinx
```

```py
import requests
import json

url = 'http://127.0.0.1:5000/api/person'
headers = {'Content-Type': 'application/json'}

filters = [dict(name='name', op='like', val='%y%')]
params = dict(q=json.dumps(dict(filters=filters)))

response = requests.get(url, params=params, headers=headers)
assert response.status_code == 200
print(response.json())


var filters = [{"name": "id", "op": "like", "val": "%y%"}]
$.ajax({
  url: 'http://127.0.0.1:5000/api/person',
  data: {"q": JSON.stringify({"filters": filters})},
  dataType: "json",
  contentType: "application/json",
  sucess: function(data) { console.log(data.objects); }
});


from flask import Flask
from flask.ext.restless import APIManager

def add_cors_headers(response):
  response.headers['Access-Control-Allow-Origin'] = 'example.com'
  response.headers['Access-Control-Allow-Credentials'] = 'true'
  return response
  
app = Flask(__name__)
manager = APIManager(app)
blueprint = manager.create_api(Person)
blueprint.after_request(add_cors_headers)

from flask import Flask
from flask.ext.restless import APIManager
from flask.ext.restless import NO_CHANGE
from flask.ext.restless import ProcessingException
from flask.ext.login import current_user
from mymodels import User

def auth_func(*args, **kwargs):
  if not current_user.is_authenticated():
    raise ProcessingException(description='Not authenticated!', code=401)
  return True

app = Flask(__name__)
api_manager = APIManager(app)
api_manager.create_api(User, preprocessors=dict(GET_SINGLE=[auth_func],
  GET_MANY=[auth_func]))


class Group(Base):
  __tablename__ = 'group'
  id = Column(Integer, primary_key=True)
  groupname = Column(Unicode)
  people = relationship('Person')

class Person(Base):
  __tablename__ = 'person'
  id = Column(Integer, primary_key=True)
  group_id = Column(Integer, ForeignKey('group.id'))
  group = relationship('Group')

  @classmethod
  def query(cls):
    original_query = session.query(cls)
    condition = (Group.groupname == 'students')
    return original_query.join(Group).filter(condition)

def preprocessor(search_params=None, **kw):
  if search_params is None:
    return
  filt = dict(name='id', op='neq', val=1)
  if 'filters' not in search_params:
    search_params['filters'] = []
  search_params['filters'].append(filt)
  
apimanager.create_api(Person, preprocessors=dict(GET_MANY=[preprocessor]))


from flask import Flask
from flask.ext.restless import APIManager
from flask.ext.restless import ProcessingException
from flask.ext.login import current_user
from mymodels import User
from mymodels import session

def auth_func(*args, **kw):
  if not current_user.is_authenticated():
    raise ProcessingException(description='Not authenticated!', code=401)

app = Flask(__name__)
api_manager = APIManager(app, session=session,
  preprocessors=dict(POST=[auth_func]))
api_manager.create_api(User)


def check_auth(instance_id=None, **kw):
  current_user = ...
  if not is_authorized_to_modify(current_user, instance_id):
    raise ProcessingException(description='Not Authorized',
      code=401)
manager.create_api(Person, preprocessors=dict(GET_SINGLE=[check_auth]))


def delete_single_preprocessor(instance_id=None, **kw):
  pass
  
def delete_postprocessor(was_deleted=None, **kw):
  pass
  
def delete_many_preprocessor(search_params=None, **kw):
  pass
  
def delete_many_postprocessor(result=None, search_params=None, **kw):
  pass

def post_preprocessor(data=None, **kw):
  pass
  
def post_postprocessor(result=None, **kw):
  pass

def post_preprocessor(data=None, **kw):
  pass
  
def post_postprocessor(result=None, **kw):
  pass
  
def patch_postprocessor(query=None, data=None, search_params=None, **kw):
  pass

def patch_many_preprocessor(search_params=None, data=None, **kw):
  pass
  
def patch_single_preprocessor(instance_id=None, data=None, **kw):
  pass
  
def patch_single_postprocessor(result=None, **kw):
  pass

def get_single_preprocessor(instance_id=None, **kw):
  pass
  
def get_single_postprocessor(result=None, **kw):
  pass

def get_many_preprocessor(search_params=None, **kw):
  pass
  
def get_many_postprocessor(result=None, search_params=None, **kw):
  pass

def pre_get_single(**kw): pass
def pre_get_many(**kw): pass
def post_patch_many(**kw): pass
def pre_delete(**kw): pass

manager.create_api(Person,
  methods=['GET', 'PATCH', 'DELETE'],
  allow_patch_many=True,
  preprocessors={
    'GET_SINGLE': [pre_get_single],
    'GET_MANY': [pre_get_many],
    'DELETE': [pre_delete]
  },
  postprocessors={
    'PATCH_MANY': [post_patch_many]
  } )


apimanager.create_api(Person, results_per_page=2)

class Person(Base):
  __tablename__ = 'person'
  id = Column(Integer, primary_key=True)
  name = Column(Unicode)
  age = Column(Integer)
  
  def name_and_age(self):
    return "%s (aged %d)" % (self.name, self.age)

include_method = ['name_and_age']
manager.create_api(Person, include_methods=['name_and_age'])
  
includes = ['name', 'birth_date', 'computers', 'computers.vendor']
apimanager.create_api(Person, include_columns=includes)

includes = ['name', 'birth_date', 'computers.vendor']
apimanager.create_api(Person, include_columns=includes)

class Person(db.Model):
  id = db.Column(db.Integer, primary_key=True)
  name= db.Column(db.Unicode, unique=True)
  birth_date = db.Column(db.Date)
  computers = db.relationship('Computer')

apimanager.create_api(Person, include_columns=['name', 'birth_date'])

apimanager.create_api(Person, exclude_columns=['name', 'birth_date'])

from cool_validation_framework import ValidationError
apimanager.create_api(Person, validation_exceptions=[ValidationError])

class PersonSchema(Schema):
  id = fields.Integer()
  name = fields.String()
  
  def make_object(self, data):
    print('MAKING OBJECT FROM', data)
    return Person(**data)
    
person_schema = PersonSchema()

def person_serializer(instance):
  return person_schema.dump(instance).data

def person_deserializer(data):
  return person_schema.load(data).data

manager = APIManager(app, session=session)
manager.create_api(Person, methods=['GET', 'POST'],
  serializer=person_seializer,
  deserializer=person_deserializer)

apimanager.create_api(Person, collection_name='people')

manager.create_api(User, primary_key='username')

apimanager.create_api(Person, methods=['PATCH'], allow_patch_many=True)

apimanager.create_api(Person, url_prefix='/api/v2')

apimanager.create_api(Person, methods=['GET', 'POST', 'DELETE'])


```
