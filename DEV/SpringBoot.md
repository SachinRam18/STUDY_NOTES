# Spring Boot CRUD Notes

## 1. Overall architecture

```text
HTTP Request
    ↓
Controller
    ↓
Service
    ↓
Repository
    ↓
DB
```

Why separate these?
- Controller → Handles HTTP requests.
- Service → Contains business logic.
- Repository → Talks to the database.
- Entity → Represents a database table.

## 2. Entity — Student
Suppose our database has a `students` table:

| id | name | age | department |
|---|---|---:|---|
| 1 | Sachin | 21 | CSBS |
| 2 | Rahul | 22 | CSE |

Create the entity like this:

```java
@Entity
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private int age;
    private String department;

    // getters and setters
}
```

What happens here?

- `@Entity` tells Spring/JPA that this Java class represents a database table.
- So `Student` becomes something like `student` table.
- `@Id` marks the primary key.

```java
@Id
private Long id;
```

This means `id` is the primary key.

## 3. Repository
Now we need something that can communicate with the database:

```java
public interface StudentRepository extends JpaRepository<Student, Long> {
}
```

This tiny interface gives us many database operations automatically, such as:

- `save()`
- `findAll()`
- `findById()`
- `deleteById()`
- `existsById()`

You do not have to write SQL for basic CRUD.

## 4. Service

```java
@Service
public class StudentService {

    private final StudentRepository repository;

    public StudentService(StudentRepository repository) {
        this.repository = repository;
    }

    public Student createStudent(Student student) {
        return repository.save(student);
    }

    public List<Student> getAllStudents() {
        return repository.findAll();
    }

    public Student getStudentById(Long id) {
        return repository.findById(id)
                .orElse(null);
    }

    public Student updateStudent(Long id, Student student) {

        Student existing = repository.findById(id)
                .orElse(null);

        if (existing == null) {
            return null;
        }

        existing.setName(student.getName());
        existing.setAge(student.getAge());
        existing.setDepartment(student.getDepartment());

        return repository.save(existing);
    }

    public void deleteStudent(Long id) {
        repository.deleteById(id);
    }
}
```

The service is basically saying:

> "When someone asks for a CRUD operation, what should I actually do?"

## 5. Controller
Now expose these operations as APIs.

```java
@RestController
@RequestMapping("/students")
public class StudentController {

    private final StudentService service;

    public StudentController(StudentService service) {
        this.service = service;
    }

    @PostMapping
    public Student createStudent(@RequestBody Student student) {
        return service.createStudent(student);
    }

    @GetMapping
    public List<Student> getAllStudents() {
        return service.getAllStudents();
    }

    @GetMapping("/{id}")
    public Student getStudent(@PathVariable Long id) {
        return service.getStudentById(id);
    }

    @PutMapping("/{id}")
    public Student updateStudent(
            @PathVariable Long id,
            @RequestBody Student student) {

        return service.updateStudent(id, student);
    }

    @DeleteMapping("/{id}")
    public void deleteStudent(@PathVariable Long id) {
        service.deleteStudent(id);
    }
}
```

Now you have a complete CRUD API.

## 6. CREATE — POST
Suppose React or Postman sends this request:

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

Spring receives it here:

```java
@PostMapping
public Student createStudent(@RequestBody Student student)
```

`@RequestBody` is important because it converts JSON into a Java object:

```java
Student student
```

Then:

```java
return service.createStudent(student);
```

goes to:

```java
repository.save(student);
```

JPA generates SQL roughly equivalent to:

```sql
INSERT INTO students (name, age, department)
VALUES ('Sachin', 21, 'CSBS');
```

The database generates the ID.

## 7. READ — GET
Get all students:

```http
GET /students
```

Controller:

```java
@GetMapping
public List<Student> getAllStudents() {
    return service.getAllStudents();
}
```

Service:

```java
public List<Student> getAllStudents() {
    return repository.findAll();
}
```

Repository:

```java
findAll()
```

JPA performs something equivalent to:

```sql
SELECT * FROM students;
```

Response:

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

## 8. READ one student
Request:

```http
GET /students/1
```

The `1` is captured by:

```java
@PathVariable Long id
```

So:

```java
@GetMapping("/{id}")
public Student getStudent(@PathVariable Long id)
```

gets:

```java
id = 1
```

Then:

```java
repository.findById(id)
```

roughly performs:

```sql
SELECT *
FROM students
WHERE id = 1;
```

## 9. UPDATE — PUT
Suppose we want to change Sachin's department.

Request:

```http
PUT /students/1
```

Body:

```json
{
  "name": "Sachin",
  "age": 21,
  "department": "CSE"
}
```

Controller:

```java
@PutMapping("/{id}")
public Student updateStudent(
        @PathVariable Long id,
        @RequestBody Student student) {

    return service.updateStudent(id, student);
}
```

Service first finds the existing student:

```java
Student existing = repository.findById(id)
        .orElse(null);
```

Then modifies it:

```java
existing.setName(student.getName());
existing.setAge(student.getAge());
existing.setDepartment(student.getDepartment());
```

Then:

```java
repository.save(existing);
```

JPA performs an update similar to:

```sql
UPDATE students
SET name = 'Sachin',
    age = 21,
    department = 'CSE'
WHERE id = 1;
```

## 10. DELETE
Request:

```http
DELETE /students/1
```

Controller:

```java
@DeleteMapping("/{id}")
public void deleteStudent(@PathVariable Long id) {
    service.deleteStudent(id);
}
```

Service:

```java
public void deleteStudent(Long id) {
    repository.deleteById(id);
}
```

JPA performs roughly:

```sql
DELETE FROM students
WHERE id = 1;
```

Student 1 is removed.

## 11. The most important thing to understand
When you see:

```java
repository.save(student);
```

don't think:

> "Where is the SQL?"

Spring Data JPA generates the database interaction for you.
That is the power of:

```java
JpaRepository<Student, Long>
```

You get:

- `save()` → INSERT / UPDATE
- `findAll()` → SELECT *
- `findById()` → SELECT WHERE id = ?
- `deleteById()` → DELETE WHERE id = ?

## 12. Full flow for an API call
Suppose React does:

```javascript
fetch("http://localhost:8080/students", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    name: "Sachin",
    age: 21,
    department: "CSBS"
  })
});
```

The flow is:

```text
React
  │
  │ POST /students
  │ JSON
  ↓
Controller
  │
  │ @RequestBody
  ↓
Student object
  │
  ↓
Service
  │
  │ createStudent()
  ↓
Repository
  │
  │ save()
  ↓
JPA / Hibernate
  │
  │ SQL
  ↓
Database
```

And the response travels back:

```text
Database
   ↓
Hibernate
   ↓
Repository
   ↓
Service
   ↓
Controller
   ↓
JSON Response
   ↓
React
```

## 13. What each annotation means
These are the ones you should definitely know for interviews:

| Annotation | Purpose |
|---|---|
| `@Entity` | Java class → database table |
| `@Id` | Primary key |
| `@GeneratedValue` | Automatically generate ID |
| `@Repository` | Database layer |
| `@Service` | Business logic layer |
| `@RestController` | REST API controller |
| `@RequestMapping` | Base URL |
| `@GetMapping` | GET API |
| `@PostMapping` | POST API |
| `@PutMapping` | PUT API |
| `@DeleteMapping` | DELETE API |
| `@RequestBody` | JSON → Java object |
| `@PathVariable` | Get value from URL |
| `@RequestParam` | Get query parameter |

For example:

```text
/students/10
          ↑
      @PathVariable
```

while:

```text
/students?name=Sachin
          ↑
      @RequestParam
```

## 14. The interview answer
If an interviewer asks:

> "How does CRUD work in Spring Boot?"

You can answer:

> "In Spring Boot, CRUD is generally implemented using a layered architecture. The Controller handles HTTP requests, the Service contains business logic, and the Repository communicates with the database using Spring Data JPA. We define an Entity to represent the database table and extend JpaRepository to get methods like save, findAll, findById and deleteById. POST is used for create, GET for read, PUT for update and DELETE for delete."

Then explain the flow:

```text
Client
 ↓
Controller
 ↓
Service
 ↓
Repository
 ↓
JPA/Hibernate
 ↓
Database
```

That flow is the main thing you should understand. Once this is clear, Spring Boot CRUD becomes much easier.


