# Technical Handoff: Task Management System

## 1. Architecture Overview
The system follows a classic client-server architecture with a clear separation of concerns.

### System Diagram
```mermaid
graph TD
    User((User)) -->|Interacts| Browser[Web Browser]
    
    subgraph Frontend [React Application]
        Browser -->|UI Events| Components[React Components]
        Components -->|API Calls| API_Service[API Service /services/api.js]
    end
    
    subgraph Backend [Express.js Server]
        API_Service -->|HTTP Requests| Server[Server server.js]
        Server -->|Routing| Router[Router routes/tasks.js]
        Router -->|SQL Queries| DB_Layer[Database Layer db.js]
    end
    
    subgraph Database [SQLite]
        DB_Layer -->|Persist/Retrieve| SQLite[(tasks.db)]
    end
```

### Component Hierarchy
- **App.jsx**: Root entry managing global state and routing.
- **TaskForm.jsx**: Input component for task creation.
- **TaskList.jsx**: Grid container for task display.
- **TaskCard.jsx**: Functional unit for individual task actions (Delete, Toggle).
- **FilterBar.jsx**: State controller for view filtering.

## 2. API Communication
The frontend communicates with an Express backend via RESTful endpoints.

### Sequence Flow (Create Task)
```mermaid
sequenceDiagram
    participant U as User
    participant F as TaskForm
    participant A as App.jsx
    participant S as API Service
    participant B as Backend API
    participant D as SQLite DB

    U->>F: Enter Title & Description
    F->>F: Validate Input
    F->>A: onSubmit(taskData)
    A->>S: api.createTask(taskData)
    S->>B: POST /api/tasks
    B->>D: INSERT INTO tasks...
    D-->>B: Return New Task Record
    B-->>S: 201 Created (JSON Task)
    S-->>A: Return Response
    A->>S: api.getTasks() (Refetch)
    S->>B: GET /api/tasks
    B-->>A: New Task List
    A->>A: Update `tasks` State
```

## 3. Code Quality & Security
- **Strengths**: Clean React hooks implementation, modern Tailwind CSS styling.
- **Security**: Prepared statements in SQLite for SQLi protection.
- **Recommendations**: Restrict CORS origins, move API URLs to environment variables, and implement XSS sanitization.
