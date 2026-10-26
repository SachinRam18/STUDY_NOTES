# Interview Revision Notes

## Spring Boot

### Inversion of Control and Dependency Injection

- **Inversion of Control (IoC)** is a design principle in which the framework controls object creation and management.
- **Dependency Injection (DI)** is a technique for providing a class with the dependencies it needs.

In a manually configured Spring application, objects might be created directly:

```java
Student student = new Student();
Address address = new Address();
```

This can create tight coupling, making testing and maintenance harder.

In Spring Boot, the IoC container creates and manages objects (called **beans**) and injects their dependencies. For example, `@Autowired` can inject a Spring-managed bean into a class.

```text
IoC container: creates Student and Address
Dependency injection: provides Address to Student
```

### How the IoC Container Works

`@SpringBootApplication` enables auto-configuration and component scanning. Component scanning discovers bean classes, after which Spring creates and manages the beans and their lifecycle.

The container's key responsibilities and internal structures include:

1. **BeanFactory** — creates and manages beans.
2. **Bean definitions** — store metadata about beans.
3. **Object creation** — creates objects in the JVM heap.
4. **Singleton cache** — stores references to singleton bean instances.
5. **Dependency injection and lifecycle management** — injects dependencies and manages bean lifecycles.

### Starting a Spring Boot Application

```java
@SpringBootApplication
public class MyApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }
}
```

- `@SpringBootApplication` enables auto-configuration and component scanning.
- `MyApplication` is the main application class.
- `main()` is the Java entry point.
- `SpringApplication.run()` starts Spring Boot and creates the IoC container.
- `MyApplication.class` and `args` provide the application class and command-line arguments.

### JPA and Hibernate

- **JPA (Java Persistence API)** is a Java specification for mapping Java objects to database tables (ORM, or Object-Relational Mapping).
- **Hibernate** is an implementation of JPA.
- **Entity** — `@Entity` maps a Java class to a database table.
- **Repository** — `JpaRepository` provides built-in CRUD operations.
- **Persistence flow:** Java entity → JPA → Hibernate → JDBC → database.

```java
@Entity
class Student {
    @Id
    private int id;

    private String name;
}
```

For example, when a student is saved:

1. The Java `Student` object is passed to the persistence API provided by Spring Data JPA.
2. Hibernate generates SQL, such as `INSERT INTO student VALUES (1, 'Sachin')`.
3. JDBC sends the SQL to MySQL.
4. MySQL stores `(1, 'Sachin')` as a row in the `student` table.

### Autowiring Beans

`@Autowired` asks Spring to inject a required Spring-managed bean.

```java
@Component
class Address {}

@Component
class Student {
    @Autowired
    Address address;
}
```

### CORS Configuration

This `WebMvcConfigurer` bean allows the specified frontend origin to make the listed requests:

```java
@Bean
WebMvcConfigurer corsConfig() {
    return new WebMvcConfigurer() {
        @Override
        public void addCorsMappings(CorsRegistry registry) {
            registry.addMapping("/**")
                .allowedOrigins("http://localhost:5173")
                .allowedMethods("GET", "POST", "PUT", "DELETE");
        }
    };
}
```

## FastAPI

### Common Use Cases

- Type hints and request validation with Pydantic.
- Automatic API documentation.
- Building APIs for machine-learning services.

### Asynchronous Requests

While waiting for an API response, an asynchronous FastAPI operation can allow the server to process other requests.

```python
async def get_data():
    result = await fetch_data()
    return result
```

### Pydantic Validation

Pydantic validates and parses data using Python type hints.

```python
from pydantic import BaseModel


class Student(BaseModel):
    name: str
    age: int
```

- `name: str` requires the name to be a string.
- `age: int` requires the age to be an integer or a value that can be parsed as one.
- Invalid request input normally results in a validation error (HTTP 422).

### Path Parameters and Query Parameters

A value included in the URL path is a **path parameter**:

```python
@app.get("/students/{id}")
def get_student(id: int):
    return {"id": id}
```

Example request: `/students/10`

A value supplied after `?` in the URL is a **query parameter**:

```python
@app.get("/students")
def get_students(age: int):
    return {"age": age}
```

Example request: `/students?age=20`

### Middleware

Middleware is code that runs between receiving a request and sending the response. It is commonly used for logging, authentication, and CORS.

An authentication dependency can check a request header:

```python
def auth(token: str = Header()):
    if token != "secret":
        raise HTTPException(401)
```

CORS middleware can be configured as follows:

```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:5173"],
    allow_methods=["*"],
    allow_headers=["*"],
)
```

### Swagger UI and OpenAPI Documentation

FastAPI creates an OpenAPI schema from routes, types, and models.

- `/docs` — interactive Swagger UI.
- `/redoc` — ReDoc documentation.
- `/openapi.json` — OpenAPI schema in JSON.

For example, defining an `@app.get("/hello")` route registers it with FastAPI, which includes it in the generated documentation at `/docs`.

## Retrieval-Augmented Generation (RAG)

### RAG Types

- **Naive RAG** — basic retrieval followed by LLM generation.
- **Advanced RAG** — improves retrieval using query rewriting, filtering, and reranking.
- **Corrective RAG (CRAG)** — evaluates and corrects poor retrieval results.
- **Adaptive RAG** — chooses a retrieval strategy based on query complexity.
- **Agentic RAG** — uses agents for iterative search and tool calls.
- **Graph RAG** — uses knowledge graphs and relationships.

### Chunking Types

- **Fixed-size** — splits text into predefined sizes.
- **Recursive** — splits using paragraphs, lines, and characters.
- **Semantic** — splits based on meaning.
- **Sliding-window** — creates overlapping chunks.
- **Parent-child** — retrieves small chunks with broader parent context.

### Retrieval Types

- **Dense retrieval** — semantic search using embeddings.
- **Sparse retrieval (BM25)** — keyword-based search.
- **Hybrid retrieval** — combines dense and sparse search.
- **Metadata filtering** — filters by source, date, or category.
- **Multi-query retrieval** — searches multiple variations of a query.
- **Multi-hop retrieval** — retrieves information across multiple connected steps.

### Ranking and Reranking

- **Cosine similarity** — measures vector similarity by angle.
- **Dot product** — scores vectors using their dot product.
- **Cross-encoder reranking** — rescores query-document pairs for relevance.
- **Maximal Marginal Relevance (MMR)** — balances relevance and diversity.
- **Reciprocal Rank Fusion (RRF)** — combines rankings from multiple search systems.

### Embedding Types

- **Dense embeddings** — dense numerical vectors representing meaning.
- **Sparse embeddings** — sparse vectors representing terms or lexical features.
- **Multimodal embeddings** — represent text, images, or other modalities.

### Vector Databases

- **FAISS** — a vector similarity search library.
- **ChromaDB** — stores embeddings and metadata.
- **Pinecone** — a managed vector database.
- **Qdrant** — a vector database with filtering.
- **pgvector** — vector search inside PostgreSQL.

### Vector Index Types

- **Flat** — exact search across all vectors.
- **HNSW** — graph-based approximate search.
- **IVF** — cluster-based approximate search.
- **PQ** — compresses vectors to reduce memory usage.

### Search Types

- **Semantic search** — searches by meaning.
- **Keyword search** — searches by words.
- **Hybrid search** — combines semantic and keyword search.
- **ANN (Approximate Nearest Neighbor)** — finds approximate nearest neighbors.
- **Top-K** — returns the K highest-ranked results.

### Advanced RAG Strategies

- **Query rewriting** — improves the search query.
- **Query decomposition** — breaks complex questions into sub-questions.
- **HyDE** — uses hypothetical relevant text to improve retrieval.
- **Reranking** — reorders retrieved chunks.
- **Context compression** — removes irrelevant context.
- **Citation grounding** — connects answers to source passages.

### Evaluation Metrics

- **Precision@K** — fraction of retrieved results that are relevant.
- **Recall@K** — fraction of relevant results successfully retrieved.
- **MRR (Mean Reciprocal Rank)** — evaluates the rank of the first relevant result.
- **NDCG (Normalized Discounted Cumulative Gain)** — measures ranking quality.
- **Faithfulness** — checks whether the answer is supported by context.
- **Answer relevance** — checks whether the answer addresses the question.

## Operating Systems

### Boot Process

1. **Power on** — the CPU starts executing firmware instructions.
2. **BIOS/UEFI** — initializes hardware, performs POST (a hardware check), and identifies a bootable device.
3. **Bootloader** — loads the OS kernel from storage into RAM and transfers control to it.
4. **Kernel initialization** — initializes memory management, process management, device drivers, and CPU scheduling.
5. **OS startup** — starts initial system processes and essential background services; the OS becomes ready.

**Flow:** Power on → BIOS/UEFI → Bootloader → Kernel initialization → OS startup

The **kernel** is the core part of the OS. It manages CPU, memory, processes, and hardware, and initializes essential OS components.

### Process Control Block (PCB)

A PCB is a data structure in kernel memory that holds information for one particular process, including:

- PID (process ID)
- Process state
- Program counter
- CPU registers
- CPU scheduling information
- Memory management information
- I/O status information
- Accounting information

### Process Memory

The memory space associated with a process in virtual memory includes:

- Code / text segment
- Data segment
- Heap
- Stack

### Process Table

The process table is an OS-maintained collection of records or references that tracks processes. It can include:

- PID
- Process state
- PCB or process reference
- Parent PID
- Scheduling information
- Resource information

### Process States and Transitions

Common process states are:

- New
- Ready
- Running
- Waiting / blocked
- Terminated

Typical transitions include:

- **New → Ready:** the process is admitted for execution.
- **Ready → Running:** the scheduler assigns CPU time.
- **Running → Ready:** CPU time expires or the process is preempted.
- **Running → Waiting:** the process waits for I/O or another event.
- **Waiting → Ready:** the required event completes.
- **Running → Terminated:** the process finishes or exits.

### System Call Flow

For example, an application can request file data using `read()`:

1. **Application (user mode):** calls `read()` to request file data.
2. **System call:** the `read()` library function executes a special CPU instruction to request an OS service.
3. **CPU transition:** the CPU switches to kernel mode and jumps to the system call handler, whose code is already in kernel memory.
4. **System call handler:** identifies the requested operation (`read`) and invokes the appropriate OS code to read the file.
5. **Return:** the kernel returns the result to the application and switches back to user mode.

### Thrashing

**Thrashing** is a condition in a virtual memory system where the OS experiences excessive page faults and spends most of its time transferring pages between RAM and disk, leaving little time for actual process execution.

**Scenario:** RAM becomes insufficient for active processes → required pages are missing from RAM → frequent page faults occur → the OS repeatedly swaps pages between RAM and disk → system performance drops drastically.

Ways to reduce thrashing include:

- Increase RAM.
- Reduce the degree of multiprogramming (the number of active processes).
- Allocate sufficient frames to processes.
- Use working-set or page-fault frequency techniques to control memory allocation.

### Belady's Anomaly

**Belady's Anomaly** is a condition in which increasing the number of page frames allocated to a process causes the number of page faults to increase instead of decrease. It can occur with the FIFO (First-In, First-Out) page replacement algorithm.

**Scenario:** a process runs with 3 page frames and experiences a certain number of page faults. Increasing the number of frames to 4 while using FIFO can unexpectedly increase the number of page faults.

- LRU (Least Recently Used) and Optimal Page Replacement do not suffer from Belady's Anomaly.
- FIFO can exhibit the anomaly because it replaces the page that entered memory earliest, regardless of how recently it was used.

### Processes and Threads

| Process | Thread |
| --- | --- |
| An independent program under execution, managed by the OS. | The smallest unit of CPU scheduling that runs within a process. |
| Has its own address space, including code, data, stack, and heap. | Shares code, data, and heap with other threads in the same process; has its own stack and registers. |
| Communicates with other processes using IPC methods, which are generally slower. | Communicates through shared memory, so communication is faster. |
| Context switching is heavier because the complete process state must be saved and restored. | Context switching is lighter because only thread-specific information is switched. |

### Inter-Process Communication (IPC)

**IPC** is a mechanism that allows separate processes to exchange data and coordinate with each other.

For example, a Chrome process may need to communicate with another process. Since processes cannot directly access each other's private memory, the OS provides IPC mechanisms to exchange data.

Common IPC mechanisms include:

- **Pipes** — transfer data between related processes.
- **Named pipes (FIFO)** — let processes communicate through a named channel.
- **Message queues** — exchange data through queued messages.
- **Shared memory** — lets multiple processes access a shared memory region.
- **Semaphores** — coordinate access to shared resources.
- **Sockets** — enable communication between processes on the same or different computers.
- **Signals** — notify a process that an event has occurred.

**Remember:** IPC means communication between processes. Shared memory and message passing are two major approaches.

### Concurrency and Parallelism

**Concurrency** means switching between tasks so that both make progress. For example, one CPU core may work on Chrome for a short time, switch to VS Code, and then switch back. On that core, only one task executes at a time.

**Parallelism** means executing tasks at the same time. For example, with two CPU cores, one core can execute Chrome while the other executes VS Code.

### Kernel

The kernel is the core component of an operating system that manages communication between software and hardware. It controls resources such as the CPU, memory, and input/output devices, helping programs run efficiently and securely.

The kernel:

- Manages the CPU, memory, and I/O devices.
- Acts as a bridge between applications and hardware.
- Provides essential services through system calls and process management.
