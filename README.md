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


