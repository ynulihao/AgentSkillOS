---
name: diagramming
description: "Create technical diagrams using Mermaid syntax. Use when visualizing system architecture (C4 model), documenting API request flows (sequence diagrams), modeling database schemas (ERDs), mapping decision logic (flowcharts), or defining state machines. Produces markdown-embedded diagrams for documentation and design reviews."
---

# Diagramming

## Workflow

1. **Select diagram type** -- Match the visualization need to the right Mermaid diagram (see table)
2. **Define scope** -- One diagram per concept; split complex systems into levels
3. **Draft structure** -- Identify nodes, relationships, and labels
4. **Write Mermaid syntax** -- Use the examples below as templates
5. **Validate** -- Render in a Mermaid-compatible viewer; check readability and label clarity

## Diagram Type Selection

| Diagram Type | Use When |
|---|---|
| C4 Context/Container | Showing system boundaries, services, and external dependencies |
| Sequence | Documenting request flows, API interactions, async processing |
| ERD | Modeling database tables, relationships, and cardinality |
| Flowchart | Mapping decision logic, CI/CD pipelines, process flows |
| State | Defining state machines, order lifecycles, connection states |
| Class | Documenting object hierarchies and interfaces |

## Principles

- One diagram, one concept -- split large diagrams into levels
- Always label relationships and transitions with descriptive text
- Use consistent naming across related diagrams
- Prefer left-to-right or top-to-bottom flow for readability
- Include a title for context

## Examples

### Architecture Diagrams (C4 Model)

#### Context Diagram (Level 1)

```mermaid
C4Context
    title System Context Diagram for E-Commerce Platform

    Person(customer, "Customer", "A user who purchases products")
    Person(admin, "Admin", "Manages products and orders")

    System(ecommerce, "E-Commerce Platform", "Allows customers to browse and purchase products")

    System_Ext(payment, "Payment Gateway", "Handles payment processing")
    System_Ext(shipping, "Shipping Provider", "Handles order fulfillment")
    System_Ext(email, "Email Service", "Sends notifications")

    Rel(customer, ecommerce, "Browses, purchases")
    Rel(admin, ecommerce, "Manages")
    Rel(ecommerce, payment, "Processes payments")
    Rel(ecommerce, shipping, "Creates shipments")
    Rel(ecommerce, email, "Sends emails")
```

#### Container Diagram (Level 2)

```mermaid
C4Container
    title Container Diagram for E-Commerce Platform

    Person(customer, "Customer")

    Container_Boundary(ecommerce, "E-Commerce Platform") {
        Container(web, "Web Application", "React", "Customer-facing UI")
        Container(api, "API Gateway", "Node.js", "REST API")
        Container(cart, "Cart Service", "Node.js", "Shopping cart management")
        Container(catalog, "Catalog Service", "Python", "Product catalog")
        Container(order, "Order Service", "Java", "Order processing")
        ContainerDb(db, "Database", "PostgreSQL", "Stores all data")
        ContainerQueue(queue, "Message Queue", "RabbitMQ", "Async messaging")
    }

    Rel(customer, web, "Uses", "HTTPS")
    Rel(web, api, "Calls", "JSON/HTTPS")
    Rel(api, cart, "Routes to")
    Rel(api, catalog, "Routes to")
    Rel(api, order, "Routes to")
    Rel(cart, db, "Reads/writes")
    Rel(catalog, db, "Reads")
    Rel(order, db, "Reads/writes")
    Rel(order, queue, "Publishes events")
```

#### Component Diagram (Level 3)

```mermaid
flowchart TB
    subgraph "Order Service"
        controller[Order Controller]
        service[Order Service]
        repo[Order Repository]
        validator[Order Validator]
        publisher[Event Publisher]
    end

    subgraph "External"
        db[(PostgreSQL)]
        queue[RabbitMQ]
    end

    controller --> service
    service --> validator
    service --> repo
    service --> publisher
    repo --> db
    publisher --> queue
```

### Sequence Diagrams

#### Basic Request Flow

```mermaid
sequenceDiagram
    autonumber
    participant C as Client
    participant G as API Gateway
    participant A as Auth Service
    participant S as Service
    participant D as Database

    C->>G: POST /api/resource
    G->>A: Validate token
    A-->>G: Token valid

    G->>S: Forward request
    S->>D: Query data
    D-->>S: Return results

    S-->>G: Response (200 OK)
    G-->>C: Response with data
```

#### Error Handling Flow

```mermaid
sequenceDiagram
    participant C as Client
    participant S as Service
    participant D as Database

    C->>S: POST /api/users
    S->>S: Validate input

    alt Validation fails
        S-->>C: 400 Bad Request
    else Validation passes
        S->>D: INSERT user
        alt Database error
            D-->>S: Constraint violation
            S-->>C: 409 Conflict
        else Success
            D-->>S: User created
            S-->>C: 201 Created
        end
    end
```

#### Async Processing

```mermaid
sequenceDiagram
    participant C as Client
    participant API as API
    participant Q as Queue
    participant W as Worker
    participant N as Notification

    C->>API: Submit job
    API->>Q: Enqueue job
    API-->>C: 202 Accepted (job ID)

    Note over Q,W: Async processing

    Q->>W: Dequeue job
    W->>W: Process job
    W->>N: Send notification
    N-->>C: Job complete notification
```

### Entity-Relationship Diagrams

#### Basic ERD

```mermaid
erDiagram
    USER ||--o{ ORDER : places
    USER {
        uuid id PK
        string email UK
        string name
        timestamp created_at
    }

    ORDER ||--|{ ORDER_ITEM : contains
    ORDER {
        uuid id PK
        uuid user_id FK
        decimal total
        string status
        timestamp created_at
    }

    ORDER_ITEM }|--|| PRODUCT : references
    ORDER_ITEM {
        uuid id PK
        uuid order_id FK
        uuid product_id FK
        int quantity
        decimal price
    }

    PRODUCT ||--o{ PRODUCT_CATEGORY : "belongs to"
    PRODUCT {
        uuid id PK
        string name
        text description
        decimal price
        int stock
    }

    CATEGORY ||--o{ PRODUCT_CATEGORY : contains
    CATEGORY {
        uuid id PK
        string name
        uuid parent_id FK
    }

    PRODUCT_CATEGORY {
        uuid product_id PK,FK
        uuid category_id PK,FK
    }
```

#### ERD with Relationships Explained

```mermaid
erDiagram
    CUSTOMER ||--o{ ORDER : "places (1:N)"
    ORDER ||--|{ LINE_ITEM : "contains (1:N, required)"
    PRODUCT ||--o{ LINE_ITEM : "appears in (1:N)"
    CUSTOMER }|--|| ADDRESS : "has billing (N:1, required)"
    CUSTOMER }o--o{ ADDRESS : "has shipping (N:N)"
```

Relationship notation:

- `||` exactly one
- `o|` zero or one
- `}|` one or more
- `}o` zero or more

### Flowcharts

#### Decision Logic

```mermaid
flowchart TD
    A[Start: User Login] --> B{Valid credentials?}
    B -->|Yes| C{2FA enabled?}
    B -->|No| D[Show error message]
    D --> A

    C -->|Yes| E[Send 2FA code]
    E --> F{Code valid?}
    F -->|Yes| G[Create session]
    F -->|No| H{Attempts < 3?}
    H -->|Yes| E
    H -->|No| I[Lock account]

    C -->|No| G
    G --> J[Redirect to dashboard]
    J --> K[End]
    I --> K
```

#### Process Flow

```mermaid
flowchart LR
    subgraph "CI Pipeline"
        A[Push Code] --> B[Run Tests]
        B --> C{Tests Pass?}
        C -->|Yes| D[Build Image]
        C -->|No| E[Notify Developer]
        D --> F[Push to Registry]
    end

    subgraph "CD Pipeline"
        F --> G[Deploy to Staging]
        G --> H[Run E2E Tests]
        H --> I{Tests Pass?}
        I -->|Yes| J[Deploy to Production]
        I -->|No| K[Rollback]
    end
```

### State Diagrams

#### Order State Machine

```mermaid
stateDiagram-v2
    [*] --> Draft: Create order

    Draft --> Pending: Submit
    Draft --> Cancelled: Cancel

    Pending --> Confirmed: Payment received
    Pending --> Cancelled: Payment failed
    Pending --> Cancelled: Timeout (24h)

    Confirmed --> Processing: Begin fulfillment
    Confirmed --> Cancelled: Customer cancel

    Processing --> Shipped: Ship order
    Processing --> Cancelled: Out of stock

    Shipped --> Delivered: Delivery confirmed
    Shipped --> Returned: Return initiated

    Delivered --> Returned: Return requested
    Delivered --> [*]: Complete

    Returned --> Refunded: Process refund
    Refunded --> [*]: Complete

    Cancelled --> [*]: Complete
```

#### Connection State Machine

```mermaid
stateDiagram-v2
    [*] --> Disconnected

    Disconnected --> Connecting: connect()
    Connecting --> Connected: success
    Connecting --> Disconnected: failure

    Connected --> Disconnected: disconnect()
    Connected --> Reconnecting: connection lost

    Reconnecting --> Connected: success
    Reconnecting --> Disconnected: max retries

    note right of Reconnecting
        Exponential backoff
        Max 5 retries
    end note
```

### Class Diagrams

```mermaid
classDiagram
    class Repository~T~ {
        <<interface>>
        +findById(id: string) T
        +findAll() List~T~
        +save(entity: T) T
        +delete(id: string) void
    }

    class UserRepository {
        -db: Database
        +findById(id: string) User
        +findAll() List~User~
        +save(entity: User) User
        +delete(id: string) void
        +findByEmail(email: string) User
    }

    class User {
        +id: string
        +email: string
        +name: string
        +createdAt: Date
        +validate() boolean
    }

    Repository~T~ <|.. UserRepository
    UserRepository --> User
```
