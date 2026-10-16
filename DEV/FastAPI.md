
# FastAPI CRUD — Complete Interview Notes

## 1. What is FastAPI?

FastAPI is a modern Python web framework used to build APIs.

It is built on:
- **Starlette** → Web and ASGI functionality.
- **Pydantic** → Data validation and serialization.
- **ASGI** → Asynchronous Server Gateway Interface.

### Why use FastAPI?
- Automatic API documentation.
- Request validation using Python type hints and Pydantic.
- Supports asynchronous programming with `async` and `await`.
- Dependency injection using `Depends()`.
- High performance for many API workloads.
- Easy integration with databases and other services.

### FastAPI vs Spring Boot

| Spring Boot | FastAPI |
|---|---|
| Java | Python |
| `@RestController` | `@app.get()`, `@app.post()` |
| `@Service` | Usually a Python service function/class |
| `JpaRepository` | SQLAlchemy session/ORM operations |
| JPA/Hibernate | SQLAlchemy ORM |
| Bean Validation | Pydantic validation |
| `@RequestBody` | Pydantic model parameter |
| `@PathVariable` | `Path` parameter or typed path argument |
| `@RequestParam` | Function parameter or `Query()` |
| `@Autowired` / constructor injection | `Depends()` for dependency injection |
| Maven/Gradle | pip/uv |
| Embedded Tomcat commonly | ASGI server such as Uvicorn |

**Important:** FastAPI does not include a built-in database repository or ORM. You can choose SQLAlchemy, SQLModel, or another database library.

---

## 2. Overall architecture

```text
Client (React / Postman / Mobile App)
                  |
                  | HTTP Request
                  v
          FastAPI Application
                  |
                  v
              Router
                  |
                  v
        Service / Business Logic
                  |
                  v
          Database Access Layer
                  |
                  v
           SQLAlchemy ORM
                  |
                  | SQL
                  v
               Database
```

### Why separate these layers?

- **Router / Controller:** Receives HTTP requests and returns HTTP responses.
- **Service:** Contains business rules and application logic.
- **Database access layer:** Performs database operations.
- **SQLAlchemy ORM:** Maps Python objects to database records and generates SQL.
- **Database:** Stores persistent data.
- **Pydantic schema:** Validates incoming data and defines the API response structure.

For a small application, these can be placed in one file. For larger applications, separating them improves maintainability and testing.

---

## 3. Install and run FastAPI

### Install dependencies

```bash
pip install fastapi uvicorn sqlalchemy
```

### Project structure

For the first example, keep everything in one file:

```text
fastapi-crud/
├── main.py
└── students.db       # Created automatically
```

### Start the server

```bash
uvicorn main:app --reload
```

Here:

- `uvicorn` → Runs the ASGI application.
- `main` → Python file `main.py`.
- `app` → FastAPI object inside that file.
- `--reload` → Restarts the server when source code changes; useful during development.

Default local address:

```text
http://127.0.0.1:8000
```

Automatic documentation:

```text
http://127.0.0.1:8000/docs
```

Alternative documentation:

```text
http://127.0.0.1:8000/redoc
```

---

## 4. Database and Entity — Student

Suppose we want to manage a `students` table.

| id | name | age | department |
|---|---|---:|---|
| 1 | Sachin | 21 | CSBS |
| 2 | Rahul | 22 | CSE |

We use SQLite for this example because it does not require a separate database server.

### SQLAlchemy model

```python
from sqlalchemy import String, Integer
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column

class Base(DeclarativeBase):
    pass

class Student(Base):
    __tablename__ = "students"

    id: Mapped[int] = mapped_column(
        Integer,
        primary_key=True,
        index=True
    )

    name: Mapped[str] = mapped_column(String(100), nullable=False)
    age: Mapped[int] = mapped_column(Integer, nullable=False)
    department: Mapped[str] = mapped_column(
        String(100),
        nullable=False
    )
```

### What happens here?

- `DeclarativeBase` → Base class for SQLAlchemy ORM models.
- `__tablename__` → Specifies the database table name.
- `Mapped[int]` → Declares the Python type of an ORM-mapped attribute.
- `mapped_column()` → Defines a database column.
- `primary_key=True` → Makes `id` the primary key.
- `nullable=False` → Prevents the column from accepting SQL `NULL`.
- `index=True` → Requests an index for the ID column.

**Interview point:** A SQLAlchemy model represents the database structure. A Pydantic schema, explained next, represents the API data contract. They serve different purposes.

---

## 5. Pydantic schemas — Request and response data

Create schemas that define which fields the API accepts and returns.

```python
from pydantic import BaseModel, ConfigDict

class StudentCreate(BaseModel):
    name: str
    age: int
    department: str

class StudentUpdate(BaseModel):
    name: str | None = None
    age: int | None = None
    department: str | None = None

class StudentResponse(BaseModel):
    model_config = ConfigDict(from_attributes=True)

    id: int
    name: str
    age: int
    department: str
```

### Why have separate schemas?

**StudentCreate**
- Used when creating a student.
- The client must provide the required fields.
- The client does not need to supply the database-generated ID.

**StudentUpdate**
- Allows clients to send only the fields they want to change.
- Used for partial updates through PATCH.

**StudentResponse**
- Defines the fields returned to the client.
- Includes the generated ID.
- `from_attributes=True` allows Pydantic to read values from SQLAlchemy model objects.

### Pydantic validation example

Suppose the client sends:

```json
{
  "name": "Sachin",
  "age": "invalid",
  "department": "CSBS"
}
```

FastAPI validates the request body against `StudentCreate`.

Because `"invalid"` cannot be converted to an integer, the request is rejected with HTTP `422 Unprocessable Entity`.

This happens before the endpoint's business logic is executed.

---

## 6. Database connection and session

Add this to `main.py`:

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, Session
from typing import Generator

DATABASE_URL = "sqlite:///./students.db"

engine = create_engine(
    DATABASE_URL,
    connect_args={"check_same_thread": False}
)

SessionLocal = sessionmaker(
    bind=engine,
    autoflush=False,
    autocommit=False
)

def get_db() -> Generator[Session, None, None]:
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

### Understand each component

- `DATABASE_URL` → Specifies where the database is located.
- `create_engine()` → Creates the SQLAlchemy engine used to communicate with the database.
- `sessionmaker()` → Creates a factory for database sessions.
- `SessionLocal()` → Creates a session for performing database operations.
- `get_db()` → Provides a session to an endpoint and closes it afterward.
- `yield` → Supplies the session to the endpoint; the `finally` block executes cleanup.

`check_same_thread=False` is a SQLite-specific setting that permits connections to be used across different threads. It does not make a SQLAlchemy session safe for concurrent use.

### Create the table

After defining the `Student` model and `engine`, run:

```python
Base.metadata.create_all(bind=engine)
```

This creates missing tables defined in the metadata. It does not automatically migrate existing tables when your model changes.

For production schema changes, use database migrations, commonly with Alembic.

---

## 7. Complete working CRUD application

The following is a complete `main.py`. It combines the database model, Pydantic schemas, database session, and CRUD endpoints.

```python
from typing import Generator

from fastapi import FastAPI, Depends, HTTPException, Response, status
from pydantic import BaseModel, ConfigDict
from sqlalchemy import String, Integer, create_engine
from sqlalchemy.orm import (
    DeclarativeBase,
    Mapped,
    mapped_column,
    sessionmaker,
    Session,
)

# ---------------- DATABASE ----------------

DATABASE_URL = "sqlite:///./students.db"

engine = create_engine(
    DATABASE_URL,
    connect_args={"check_same_thread": False}
)

SessionLocal = sessionmaker(
    bind=engine,
    autoflush=False,
    autocommit=False,
)


class Base(DeclarativeBase):
    pass


# ---------------- SQLALCHEMY MODEL ----------------

class Student(Base):
    __tablename__ = "students"

    id: Mapped[int] = mapped_column(
        Integer,
        primary_key=True,
        index=True
    )

    name: Mapped[str] = mapped_column(String(100), nullable=False)
    age: Mapped[int] = mapped_column(Integer, nullable=False)
    department: Mapped[str] = mapped_column(
        String(100),
        nullable=False
    )


Base.metadata.create_all(bind=engine)


# ---------------- PYDANTIC SCHEMAS ----------------

class StudentCreate(BaseModel):
    name: str
    age: int
    department: str


class StudentUpdate(BaseModel):
    name: str | None = None
    age: int | None = None
    department: str | None = None


class StudentResponse(BaseModel):
    model_config = ConfigDict(from_attributes=True)

    id: int
    name: str
    age: int
    department: str


# ---------------- DATABASE DEPENDENCY ----------------

def get_db() -> Generator[Session, None, None]:
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()


# ---------------- FASTAPI APPLICATION ----------------

app = FastAPI(title="Student CRUD API")


# CREATE: POST /students

@app.post(
    "/students",
    response_model=StudentResponse,
    status_code=status.HTTP_201_CREATED,
)
def create_student(
    student: StudentCreate,
    db: Session = Depends(get_db),
):
    new_student = Student(**student.model_dump())

    db.add(new_student)
    db.commit()
    db.refresh(new_student)

    return new_student


# READ ALL: GET /students

@app.get("/students", response_model=list[StudentResponse])
def get_students(db: Session = Depends(get_db)):
    return db.query(Student).all()


# READ ONE: GET /students/{id}

@app.get("/students/{student_id}", response_model=StudentResponse)
def get_student(student_id: int, db: Session = Depends(get_db)):
    student = db.get(Student, student_id)

    if student is None:
        raise HTTPException(
            status_code=404,
            detail="Student not found"
        )

    return student


# UPDATE: PUT /students/{id}
# PUT replaces all client-editable fields.

@app.put("/students/{student_id}", response_model=StudentResponse)
def update_student(
    student_id: int,
    student_data: StudentCreate,
    db: Session = Depends(get_db),
):
    student = db.get(Student, student_id)

    if student is None:
        raise HTTPException(
            status_code=404,
            detail="Student not found"
        )

    for field, value in student_data.model_dump().items():
        setattr(student, field, value)

    db.commit()
    db.refresh(student)

    return student


# PARTIAL UPDATE: PATCH /students/{id}

@app.patch("/students/{student_id}", response_model=StudentResponse)
def patch_student(
    student_id: int,
    student_data: StudentUpdate,
    db: Session = Depends(get_db),
):
    student = db.get(Student, student_id)

    if student is None:
        raise HTTPException(
            status_code=404,
            detail="Student not found"
        )

    updates = student_data.model_dump(exclude_unset=True)

    for field, value in updates.items():
        setattr(student, field, value)

    db.commit()
    db.refresh(student)

    return student


# DELETE: DELETE /students/{id}

@app.delete(
    "/students/{student_id}",
    status_code=status.HTTP_204_NO_CONTENT,
)
def delete_student(
    student_id: int,
    db: Session = Depends(get_db),
):
    student = db.get(Student, student_id)

    if student is None:
        raise HTTPException(
            status_code=404,
            detail="Student not found"
        )

    db.delete(student)
    db.commit()

    return Response(status_code=status.HTTP_204_NO_CONTENT)
```

Run it:

```bash
uvicorn main:app --reload
```

Open `http://127.0.0.1:8000/docs` to test the endpoints interactively.

**Important:** This is a learning example. For production, add transaction rollback handling, database migrations, stricter validation, configuration management, and tests.

---

## 8. CREATE — POST

Request:

```http
POST /students
Content-Type: application/json
```

Body:

```json
{
  "name": "Sachin",
  "age": 21,
  "department": "CSBS"
}
```

### Flow

```text
Client sends JSON
       ↓
FastAPI receives POST
       ↓
Pydantic validates StudentCreate
       ↓
Endpoint receives student object
       ↓
Student(**student.model_dump())
       ↓
db.add(new_student)
       ↓
db.commit()
       ↓
db.refresh(new_student)
       ↓
JSON response with generated ID
```

### Understand the database operations

```python
new_student = Student(**student.model_dump())
```

Converts the validated Pydantic data into a SQLAlchemy model instance.

```python
db.add(new_student)
```

Stages the object for insertion in the current session. It does not necessarily execute the INSERT immediately.

```python
db.commit()
```

Flushes pending changes and commits the transaction.

```python
db.refresh(new_student)
```

Reloads database-generated values, such as the generated primary key, into the Python object.

Equivalent SQL:

```sql
INSERT INTO students (name, age, department)
VALUES ('Sachin', 21, 'CSBS');
```

Possible response — HTTP `201 Created`:

```json
{
  "id": 1,
  "name": "Sachin",
  "age": 21,
  "department": "CSBS"
}
```

---

## 9. READ — GET all students

Request:

```http
GET /students
```

Endpoint:

```python
@app.get("/students", response_model=list[StudentResponse])
def get_students(db: Session = Depends(get_db)):
    return db.query(Student).all()
```

Equivalent SQL:

```sql
SELECT * FROM students;
```

Possible response — HTTP `200 OK`:

```json
[
  {
    "id": 1,
    "name": "Sachin",
    "age": 21,
    "department": "CSBS"
  },
  {
    "id": 2,
    "name": "Rahul",
    "age": 22,
    "department": "CSE"
  }
]
```

`response_model=list[StudentResponse]` tells FastAPI to validate and serialize the returned data as a list of student response objects.

For large tables, add pagination instead of returning every record.

---

## 10. READ one student

Request:

```http
GET /students/1
```

Endpoint:

```python
@app.get("/students/{student_id}", response_model=StudentResponse)
def get_student(student_id: int, db: Session = Depends(get_db)):
    student = db.get(Student, student_id)

    if student is None:
        raise HTTPException(
            status_code=404,
            detail="Student not found"
        )

    return student
```

### How does it work?

```text
GET /students/1
       ↓
student_id = 1
       ↓
db.get(Student, 1)
       ↓
SQLAlchemy looks up the primary key
       ↓
Student object or None
       ↓
JSON response or HTTP 404
```

Equivalent SQL:

```sql
SELECT * FROM students WHERE id = 1;
```

`db.get(Student, 1)` is specifically designed for primary-key lookup. SQLAlchemy may satisfy it from the session's identity map when appropriate, rather than always issuing a new query.

---

## 11. UPDATE — PUT

Request:

```http
PUT /students/1
Content-Type: application/json
```

Body:

```json
{
  "name": "Sachin",
  "age": 21,
  "department": "CSE"
}
```

Endpoint logic:

```python
student = db.get(Student, student_id)

if student is None:
    raise HTTPException(status_code=404, detail="Student not found")

for field, value in student_data.model_dump().items():
    setattr(student, field, value)

db.commit()
db.refresh(student)
```

### Flow

```text
Receive PUT request
       ↓
Validate complete request body
       ↓
Find existing student
       ↓
Return 404 if not found
       ↓
Update editable fields
       ↓
Commit transaction
       ↓
Return updated student
```

Equivalent SQL:

```sql
UPDATE students
SET name = 'Sachin',
    age = 21,
    department = 'CSE'
WHERE id = 1;
```

**Interview distinction:**

- `PUT` → Conventionally replaces the resource representation; this example replaces all editable fields.
- `PATCH` → Updates only the specified fields.

Both may be implemented using SQLAlchemy ORM operations.

---

## 12. PATCH — Partial update

Suppose you only want to change the department.

Request:

```http
PATCH /students/1
Content-Type: application/json
```

Body:

```json
{
  "department": "CSE"
}
```

The important code is:

```python
updates = student_data.model_dump(exclude_unset=True)

for field, value in updates.items():
    setattr(student, field, value)

db.commit()
db.refresh(student)
```

### Why `exclude_unset=True`?

It includes only fields explicitly supplied in the request.

For example, the client sends:

```json
{
  "department": "CSE"
}
```

The update dictionary becomes:

```python
{"department": "CSE"}
```

The student's name and age remain unchanged.

**Interview trap:** `exclude_unset=True` is different from `exclude_none=True`. Explicitly supplied `null` values are preserved by `exclude_unset=True`; whether they are allowed depends on your validation and database rules.

---

## 13. DELETE

Request:

```http
DELETE /students/1
```

Endpoint:

```python
student = db.get(Student, student_id)

if student is None:
    raise HTTPException(status_code=404, detail="Student not found")

db.delete(student)
db.commit()
```

Equivalent SQL:

```sql
DELETE FROM students WHERE id = 1;
```

The example returns HTTP `204 No Content`, meaning deletion succeeded and there is no response body.

### Why not simply use `db.delete(student_id)`?

`db.delete()` expects a mapped object, not a primary-key value. Find the student first, then delete the object.

---

## 14. What is `Depends()`? — Important interview topic

Example:

```python
def get_students(db: Session = Depends(get_db)):
    return db.query(Student).all()
```

`Depends()` is FastAPI's dependency injection mechanism.

It tells FastAPI to resolve `get_db` and pass its result into the endpoint.

### Without dependency injection

You might create a database session manually inside every endpoint, duplicating connection and cleanup logic.

### With dependency injection

```text
Incoming request
       ↓
FastAPI resolves Depends(get_db)
       ↓
get_db creates a session
       ↓
Session is injected into endpoint
       ↓
Endpoint performs database operations
       ↓
Request cleanup closes session
```

Benefits:

- Reuse common logic.
- Manage resource lifecycles.
- Simplify testing and mocking.
- Share authentication and authorization dependencies.
- Avoid repeating database setup in every endpoint.

**Important:** `Depends()` does not itself create a database connection. It asks FastAPI to resolve the dependency callable.

---

## 15. How does FastAPI convert JSON into Python objects?

Request:

```json
{
  "name": "Sachin",
  "age": 21,
  "department": "CSBS"
}
```

Endpoint:

```python
@app.post("/students")
def create_student(student: StudentCreate):
    ...
```

Flow:

```text
HTTP request body
       ↓
JSON decoding
       ↓
Pydantic validation
       ↓
StudentCreate instance
       ↓
Endpoint executes
```

FastAPI recognizes the Pydantic model parameter as request-body data.

If validation fails, FastAPI typically returns HTTP `422` with details describing the invalid fields.

If validation succeeds, the endpoint receives a Python object rather than a raw JSON string.

---

## 16. Path parameters vs query parameters vs request body

### Path parameter

```python
@app.get("/students/{student_id}")
def get_student(student_id: int):
    return {"id": student_id}
```

Request:

```http
GET /students/10
```

`student_id` becomes `10`.

Use it to identify a specific resource.

### Query parameter

```python
from fastapi import Query

@app.get("/students/search")
def search_students(
    department: str = Query(...),
    age: int | None = None,
):
    return {
        "department": department,
        "age": age,
    }
```

Request:

```http
GET /students/search?department=CSBS&age=21
```

- `department` is required because of `Query(...)`.
- `age` is optional because it defaults to `None`.

Use query parameters for filtering, sorting, pagination, and search options.

### Request body

```python
@app.post("/students")
def create_student(student: StudentCreate):
    return student
```

The JSON body contains the student data.

### Quick comparison

| Type | Example | Typical purpose |
|---|---|---|
| Path | `/students/10` | Identify a resource |
| Query | `/students?department=CSBS` | Filter or control a request |
| Body | JSON containing student data | Send structured data |

---

## 17. HTTP status codes you must know

| Status code | Meaning | Typical CRUD use |
|---|---|---|
| `200 OK` | Request succeeded | GET, successful PUT/PATCH |
| `201 Created` | Resource created | POST |
| `204 No Content` | Success with no body | DELETE |
| `400 Bad Request` | Request is invalid for the operation | Application-specific errors |
| `401 Unauthorized` | Authentication is missing or invalid | Protected endpoints |
| `403 Forbidden` | User lacks permission | Authorization failures |
| `404 Not Found` | Resource does not exist | GET, PUT, PATCH, DELETE |
| `409 Conflict` | Conflicting resource state | Duplicate unique value |
| `422 Unprocessable Entity` | Request data failed validation | Invalid Pydantic input |
| `500 Internal Server Error` | Unexpected server error | Unhandled application failure |

Note: `422` is FastAPI's default validation response in the example; HTTP semantics and application conventions may vary by API design.

---

## 18. Exception handling with `HTTPException`

Example:

```python
from fastapi import HTTPException

raise HTTPException(
    status_code=404,
    detail="Student not found"
)
```

Response:

```json
{
  "detail": "Student not found"
}
```

### Why use it?

It allows the API to return an intentional HTTP error instead of silently returning `None` or producing an unrelated server error.

In the CRUD example, a missing student results in `404 Not Found`.

Do not expose database passwords, internal stack traces, or sensitive implementation details in error responses.

---

## 19. How does FastAPI generate SQL?

In Spring Boot, `JpaRepository` provides ready-made repository methods.

In the SQLAlchemy example, you explicitly use session operations:

| SQLAlchemy | Rough SQL equivalent |
|---|---|
| `db.add(student)` + `db.commit()` | INSERT |
| `db.query(Student).all()` | SELECT all |
| `db.get(Student, 1)` | SELECT by primary key |
| Modify object + `db.commit()` | UPDATE |
| `db.delete(student)` + `db.commit()` | DELETE |

Example:

```python
db.add(new_student)
db.commit()
```

SQLAlchemy tracks the mapped object and generates the appropriate SQL when changes are flushed.

You do not normally need to write raw SQL for these basic operations.

However, SQLAlchemy is not the same as Spring Data JPA: it does not automatically provide an identical `JpaRepository` interface. SQLAlchemy offers ORM and SQL expression APIs that you use to implement your data access.

---

## 20. How does React communicate with FastAPI?

React frontend:

```javascript
async function createStudent() {
  const response = await fetch(
    "http://127.0.0.1:8000/students",
    {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify({
        name: "Sachin",
        age: 21,
        department: "CSBS"
      })
    }
  );

  if (!response.ok) {
    throw new Error(`Request failed: ${response.status}`);
  }

  const student = await response.json();
  console.log(student);
}
```

### Full request flow

```text
React
  |
  | POST /students + JSON
  v
FastAPI
  |
  | Parse and validate JSON
  v
StudentCreate (Pydantic)
  |
  v
CRUD endpoint
  |
  v
SQLAlchemy Session
  |
  v
SQL statement
  |
  v
Database
  |
  v
SQLAlchemy model object
  |
  v
Pydantic response validation
  |
  v
JSON response
  |
  v
React receives student data
```

### What if the browser reports a CORS error?

If the frontend and backend have different origins, configure CORS in FastAPI when needed.

For example, if Vite runs on port `5173`:

```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:5173"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

Place this after creating the FastAPI application.

CORS is a browser security mechanism, not a database or CRUD feature. Allow only the origins your application needs in production.

---

## 21. Decorators and concepts to know

| Decorator / concept | Purpose |
|---|---|
| `@app.get()` | Register a GET endpoint |
| `@app.post()` | Register a POST endpoint |
| `@app.put()` | Register a PUT endpoint |
| `@app.patch()` | Register a PATCH endpoint |
| `@app.delete()` | Register a DELETE endpoint |
| `Depends()` | Resolve dependencies |
| `HTTPException` | Return an intentional HTTP error |
| `Query()` | Configure query parameters |
| `Path()` | Configure path parameters |
| `Body()` | Configure request-body parameters |
| `BaseModel` | Define Pydantic schemas |
| `Field()` | Configure and validate model fields |
| `response_model` | Define and validate the response contract |
| `status_code` | Specify the endpoint's success status |
| `CORSMiddleware` | Configure cross-origin browser access |
| `async def` | Declare an asynchronous function |
| `yield` in a dependency | Provide a value and perform cleanup |

Example using `Path()`:

```python
from typing import Annotated
from fastapi import Path

@app.get("/students/{student_id}")
def get_student(
    student_id: Annotated[int, Path(gt=0)]
):
    return {"id": student_id}
```

Here, the ID must be greater than zero.

---

## 22. `def` vs `async def`

### Synchronous endpoint

```python
@app.get("/hello")
def hello():
    return {"message": "Hello"}
```

### Asynchronous endpoint

```python
@app.get("/hello-async")
async def hello_async():
    return {"message": "Hello"}
```

Both can be valid FastAPI endpoints.

### Interview explanation

- `def` declares a normal synchronous Python function.
- `async def` declares a coroutine that can use `await`.
- FastAPI runs ordinary synchronous endpoints in a thread pool.
- Asynchronous endpoints can cooperate with the event loop when awaiting non-blocking operations.
- Use an async-compatible database driver and async database APIs if you want genuinely asynchronous database access.

**Important:** Writing `async def` does not automatically make blocking database calls non-blocking. Avoid calling blocking I/O directly inside an asynchronous endpoint.

---

## 23. Authentication vs authorization

These concepts frequently appear in FastAPI interviews.

- **Authentication:** Who are you?
- **Authorization:** What are you allowed to do?

Example:

```text
Client sends access token
       ↓
FastAPI validates credentials
       ↓
Identifies the user
       ↓
Checks permissions
       ↓
Allows or rejects the operation
```

FastAPI supports security utilities and dependency-based authentication. OAuth2 and JWT are common topics, but they are separate from basic CRUD.

Never assume that hiding a button in React protects an API. Permissions must be checked on the server.

---

## 24. Common interview questions and short answers

### Q1. What is FastAPI?

FastAPI is a Python framework for building APIs. It uses type hints, Pydantic validation, and ASGI support to provide automatic documentation and efficient request handling.

### Q2. What is the difference between FastAPI and Flask?

FastAPI emphasizes type-driven validation, automatic OpenAPI documentation, and asynchronous endpoint support. Flask is a lightweight WSGI framework with a flexible extension ecosystem. Both can build APIs.

### Q3. What is Pydantic?

Pydantic validates and serializes data using Python type annotations. FastAPI uses it to validate request bodies and structure response data.

### Q4. What is SQLAlchemy?

SQLAlchemy is a Python SQL toolkit and ORM. It allows applications to work with relational databases through Python objects and SQL expressions.

### Q5. What is the difference between a SQLAlchemy model and a Pydantic model?

A SQLAlchemy model maps to database entities and tracks persistence. A Pydantic model defines and validates data entering or leaving the API.

### Q6. What is dependency injection?

Dependency injection supplies an endpoint with required components, such as a database session or authenticated user, instead of making the endpoint construct everything itself.

### Q7. What is `Depends()`?

`Depends()` declares a dependency that FastAPI resolves before invoking the endpoint.

### Q8. Why use `db.commit()`?

It commits the current transaction, making successful database changes persistent. A session may flush SQL before the commit, but flushing alone is not the same as committing.

### Q9. What does `db.refresh()` do?

It reloads the mapped object from the database so the application can access current database-generated values.

### Q10. What is `response_model`?

It declares the response schema used for validation, serialization, and API documentation. It can also help prevent unintended fields from being exposed.

### Q11. What happens when the request body is invalid?

FastAPI's Pydantic-based validation normally rejects the request with `422 Unprocessable Entity` before the endpoint runs.

### Q12. Why use `HTTPException`?

It lets the application return a deliberate HTTP error, such as `404` when a student does not exist.

### Q13. What is the difference between PUT and PATCH?

PUT conventionally replaces a resource representation; PATCH applies partial changes. The exact semantics depend on the API implementation.

### Q14. Does FastAPI automatically create database tables?

No. Table creation is handled by your ORM or database setup. `Base.metadata.create_all()` can create missing SQLAlchemy tables, while Alembic is commonly used for migrations.

### Q15. Does FastAPI require SQLAlchemy?

No. SQLAlchemy is one option. You can use SQLModel, another database library, or direct SQL.

### Q16. What is Uvicorn?

Uvicorn is an ASGI server that runs applications such as FastAPI.

### Q17. What is ASGI?

ASGI is the Asynchronous Server Gateway Interface, a standard interface between Python web servers and applications. It supports asynchronous request handling and other capabilities beyond traditional WSGI.

### Q18. What is OpenAPI?

OpenAPI is a specification for describing HTTP APIs. FastAPI generates an OpenAPI schema that powers its interactive Swagger UI and ReDoc documentation.

### Q19. How do you handle a missing student?

Look up the student and raise `HTTPException(status_code=404, detail="Student not found")` if the record does not exist.

### Q20. How would you improve this CRUD API for production?

- Use a production database and migrations.
- Add authentication and authorization.
- Validate inputs and enforce database constraints.
- Handle transaction failures and rollbacks.
- Add pagination and filtering.
- Use structured logging and appropriate error handling.
- Write unit and integration tests.
- Manage secrets through environment variables.
- Configure CORS narrowly.
- Avoid returning sensitive fields in responses.

---

## 25. Common mistakes

1. **Forgetting `db.commit()`**  
   Changes may not become persistent.

2. **Forgetting to close the database session**  
   Resources can remain occupied. Use a dependency with `try/finally`.

3. **Returning `None` for a missing record**  
   Raise a deliberate `404` when the resource does not exist.

4. **Confusing ORM models with Pydantic schemas**  
   They solve different problems.

5. **Forgetting `response_model`**  
   You lose an explicit response contract and some automatic output validation.

6. **Using `PUT` for an accidental partial update**  
   Define your API semantics clearly. Use PATCH for partial updates.

7. **Assuming `async def` makes every operation asynchronous**  
   Blocking operations can still block the event loop.

8. **Using `create_all()` as a migration system**  
   It does not manage arbitrary changes to existing schemas.

9. **Exposing every database field to clients**  
   Define response schemas carefully.

10. **Ignoring transactions and exceptions**  
    Production applications need appropriate rollback and error-handling strategies.

---

## 26. Final revision sheet

| Operation | HTTP method | Example endpoint | Database operation |
|---|---|---|---|
| Create | POST | `/students` | INSERT |
| Read all | GET | `/students` | SELECT |
| Read one | GET | `/students/1` | SELECT by primary key |
| Replace | PUT | `/students/1` | UPDATE editable fields |
| Partial update | PATCH | `/students/1` | UPDATE selected fields |
| Delete | DELETE | `/students/1` | DELETE |

### Remember this flow

```text
Client
  ↓
FastAPI Router
  ↓
Pydantic Validation
  ↓
Endpoint / Service Logic
  ↓
Database Session
  ↓
SQLAlchemy ORM
  ↓
Database
  ↓
Response Model
  ↓
JSON Response
  ↓
Client
```

### Best interview answer

"If I had to implement CRUD in FastAPI, I would define SQLAlchemy models for database persistence and Pydantic schemas for request validation and response serialization. I would create endpoints using decorators such as `@app.post`, `@app.get`, `@app.put`, `@app.patch`, and `@app.delete`. I would use dependency injection through `Depends()` to manage database sessions, SQLAlchemy operations to perform CRUD, and `HTTPException` for appropriate errors. FastAPI would validate incoming data and generate OpenAPI documentation automatically."

**The three most important concepts to master:** Pydantic validation, dependency injection with `Depends()`, and database session management with SQLAlchemy.

