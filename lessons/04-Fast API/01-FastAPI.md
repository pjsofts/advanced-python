---
title: "FAST API"
description: "FAST API Introduction"
---

Here's an example of using FastAPI along with SQLAlchemy to create an endpoint that interacts with a PostgreSQL database to manage user information (username and password).

First, you'll need to set up your environment by installing the necessary packages:

```bash
pip install fastapi uvicorn sqlalchemy databases[postgresql]
```

Here's the code for setting up FastAPI with SQLAlchemy to manage users:

```python
from fastapi import FastAPI, HTTPException, Depends
from sqlalchemy import create_engine, Column, String
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker, Session
from typing import Optional

# Replace the database_url with your PostgreSQL database URL
DATABASE_URL = "postgresql://username:password@localhost/db_name"

# SQLAlchemy configurations
engine = create_engine(DATABASE_URL)
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)
Base = declarative_base()

# Define User model
class User(Base):
    __tablename__ = "users"

    username = Column(String, primary_key=True, index=True)
    password = Column(String)

# Create tables in the database
Base.metadata.create_all(bind=engine)

app = FastAPI()

# Dependency to get the database session
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

# Create a new user
@app.post("/users/")
def create_user(username: str, password: str, db: Session = Depends(get_db)):
    hashed_password = password  # You should hash the password for security, not store it as plaintext
    db_user = User(username=username, password=hashed_password)
    db.add(db_user)
    db.commit()
    return {"username": username}

# Get user details by username
@app.get("/users/{username}")
def get_user(username: str, db: Session = Depends(get_db)):
    user = db.query(User).filter(User.username == username).first()
    if user is None:
        raise HTTPException(status_code=404, detail="User not found")
    return user
```

Please note:
- Ensure to replace `DATABASE_URL` with your actual PostgreSQL database URL.
- The code doesn't include password hashing for simplicity. In practice, you should always hash passwords before storing them in the database for security reasons.

This code creates an API with two endpoints:
- `POST /users/`: Create a new user by providing a username and password.
- `GET /users/{username}`: Retrieve user details by providing a username.

Remember to handle password hashing, validation, authentication, and other security-related concerns based on your application's requirements before deploying this code to production.