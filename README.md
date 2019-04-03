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

```
```
