---
title: "Flask Application"
description: "Flask Application"
---

Below is an example of how you might structure a Flask application with a 'users' entity using the recommended project structure:

### Folder Structure:

```
my_flask_app/
│
├── app/
│   ├── __init__.py
│   ├── models/
│   │   └── user.py
│   ├── views/
│   │   └── user_views.py
│   ├── services/
│   │   └── user_service.py
│   ├── forms/
│   │   └── user_forms.py
│   ├── static/
│   └── templates/
├── config.py
├── requirements.txt
└── run.py
```

### Code Example:

#### `app/__init__.py` (Initialization)

```python
from flask import Flask
from config import Config
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config.from_object(Config)
db = SQLAlchemy(app)

from app.views import user_views  # Importing views here to register the routes
```

#### `app/models/user.py` (User Model)

```python
from app import db

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(50), unique=True, nullable=False)
    password = db.Column(db.String(100), nullable=False)
    # Other user attributes...
```

#### `app/views/user_views.py` (User Routes)

```python
from app import app
from flask import jsonify
from app.services.user_service import UserService

@app.route('/users', methods=['GET'])
def get_users():
    users = UserService.get_all_users()
    return jsonify(users)

# Other user-related routes (e.g., create, update, delete)
```

#### `app/services/user_service.py` (User Service)

```python
from app.models.user import User

class UserService:
    @staticmethod
    def get_all_users():
        return User.query.all()

    # Implement other user-related business logic here...
```

#### `app/forms/user_forms.py` (User Forms or Schemas - for validation, serialization)

```python
from flask_wtf import FlaskForm
from wtforms import StringField, PasswordField
from wtforms.validators import DataRequired, Length

class UserForm(FlaskForm):
    username = StringField('Username', validators=[DataRequired(), Length(min=4, max=50)])
    password = PasswordField('Password', validators=[DataRequired(), Length(min=6)])
    # Other form fields...
```

#### `config.py` (Configuration File)

```python
class Config:
    SQLALCHEMY_DATABASE_URI = 'sqlite:///site.db'  # Replace with your database URI
    SQLALCHEMY_TRACK_MODIFICATIONS = False
```

#### `run.py` (Entry Point)

```python
from app import app

if __name__ == '__main__':
    app.run(debug=True)
```

This structure separates concerns by having models for the database, views for routing, services for business logic, and forms for handling user input. You can expand each file or module based on your project's requirements, adding user creation, authentication, validation, and other functionalities related to user management. Adjust the database URI and configurations according to your database setup.