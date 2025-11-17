**PROJECT 180**
**Real-World Agentic AI Backend Challenge**
NLSQL Agent • MCP Server • Role-Based Access • End-to-End API System

**SECTION 1: PROJECT OVERVIEW**
Project 180 is a backend AI engineering challenge designed to simulate real enterprise agentic systems. Participants will build a natural-language-driven backend system where an agent interprets user queries, validates user permissions, calls MCP tools, interacts with a database, and returns structured JSON results.

The system must support data retrieval, creation, updates, and deletion while using strict role-based access control. All features must be implemented through backend APIs and tested using Postman. No frontend is required.

This challenge prepares developers for real-world AI engineering involving agent pipelines, MCP integration, RBAC, database CRUD, logging, and production-ready workflows.

**SECTION 2: PROBLEM STATEMENT**
Participants must build a backend system with at least eight API endpoints covering realistic enterprise scenarios. The system must allow users to interact with the database using natural language. The agent must convert these natural language queries into structured operations, validate role permissions, and trigger the correct MCP tool.

**Mandatory API endpoints (minimum eight):**

1. POST /query – Accept natural language and trigger the agent pipeline.
2. POST /query/structured – Accept structured JSON for direct operations.
3. POST /register – Register a user with a role.
4. GET /users – Retrieve all registered users.
5. GET /audit-log – Fetch audit logs of all operations.
6. GET /mcp/health – Verify MCP server and tool availability.
7. POST /admin/seed – Seed the database with starter data (Admin only).
8. POST /admin/reset – Reset the database entirely (Admin only).

**Additional recommended endpoints:**
9. GET /roles – Show all role definitions.
10. GET /db/schema – Display current database schema.
11. POST /mcp/switch – Update MCP server URL at runtime.
12. GET /audit-log/user/{user_id} – Filter logs for a specific user.

**SECTION 3: REQUIRED SCENARIOS**
Participants must implement all of the following scenarios:

**Scenario 1: Basic Retrieval**
Example: "Show all customers from Dubai." The agent must parse filtering conditions and call the get_records tool.

**Scenario 2: Create Operation**
Example: "Add a new product called Macbook Air priced at 4500." Viewer must be denied; Editor and Admin allowed.

**Scenario 3: Update Operation**
Example: "Update the salary of employee 9 to 15000." The agent must detect the target table, condition, and update fields.

**Scenario 4: Delete Operation (Admin Only)**
Example: "Delete customer 11 from the system." Editors and Viewers must be denied.

**Scenario 5: Conditional Filtering**
Example: "Show all orders placed this month with total above 500." The agent must interpret date filters and numeric comparisons.

**Scenario 6: Sorting and Limits**
Example: "Show the latest five customers sorted by creation date descending."

**Scenario 7: Multi-Table Join**
Example: "Show all orders along with customer names." Must be handled via structured join logic or advanced MCP tool.

**Scenario 8: Bulk Insert**
Example: "Add three employees John, Sara, and Alex to the IT department." The agent must process multiple insertions.

**Scenario 9: Bulk Update**
Example: "Increase all product prices by 10 percent where category is electronics."

**Scenario 10: Complex Query**
Example: "Show customers who have placed more than two orders." Requires joins or subquery interpretation.

**Scenario 11: Error Handling**
Example: Querying a non-existent table. The agent must safely return a descriptive error.

**Scenario 12: Admin Utility Operations**
Includes resetting the database or seeding it.

**Scenario 13: Unsupported Intent**
Example: "What is the weather today?" The system must return an "intent not supported" response.

**Scenario 14: MCP Failure Handling**
If MCP server is unreachable, system must return a proper error without crashing.

**SECTION 4: HIGH-LEVEL FLOW**

1. User sends natural language to API.
2. API forwards the request to the Supervisor Agent.
3. Supervisor Agent validates user permissions.
4. NLSQL Agent converts query to structured action.
5. Supervisor selects appropriate MCP tool.
6. MCP tool performs DB operation.
7. Result returned to API.
8. Operation logged in audit log.
9. API returns final JSON response.

**SECTION 5: SUBMISSION REQUIREMENTS**

* Complete GitHub repository with clear structure.
* Fully implemented mandatory endpoints.
* Deployed MCP server (public URL or local via tunneling).
* Postman collection with at least 25 meaningful requests.
* Architecture diagram.
* Complete README explaining system behavior.
* Demonstrated RBAC, NLSQL parsing, MCP interactions, and error handling.

**SECTION 6: FINAL OBJECTIVE**
Build a production-like backend agentic system where a user sends natural language, the agent interprets it, permissions are validated, MCP tools perform the action, logs are recorded, and a clean structured response is returned.

**SECTION 7: PROJECT ARCHITECTURE (MERMAID DIAGRAM)**
The following mermaid diagram outlines the complete end-to-end architecture for Project 180, showing how user input flows through the system, how the agent interprets it, how MCP tools execute actions, and how the final response is returned.

```mermaid
digraph G {
    rankdir=LR;

    User["User (Sends Natural Language Query)"];
    API["Backend API Layer (FastAPI)"];
    Supervisor["Supervisor Agent"];
    RBAC["Role Validator (RBAC)"];
    NLSQL["NLSQL Agent (Parser)"];
    MCPClient["MCP Client"];
    MCPServer["MCP Server with Tools"];
    DB["Database (PostgreSQL/SQLite)"];
    Logs["Audit Log Store"];

    User -> API;
    API -> Supervisor;
    Supervisor -> RBAC;
    RBAC -> Supervisor;
    Supervisor -> NLSQL;
    NLSQL -> Supervisor;
    Supervisor -> MCPClient;
    MCPClient -> MCPServer;
    MCPServer -> DB;
    DB -> MCPServer;
    MCPServer -> MCPClient;
    MCPClient -> Supervisor;
    Supervisor -> API;
    API -> Logs;
    Logs -> API;
}
```

**SECTION 8: DETAILED COMPONENT RESPONSIBILITIES**
This section explains each component in production-level detail.

**1. Backend API (FastAPI)**

* Exposes all REST endpoints
* Validates request schema
* Forwards NL queries to Supervisor Agent
* Handles errors and response formatting
* Stores logs into audit-log database or file-based logger

**2. Supervisor Agent**

* Acts as the orchestrator coordinating multiple subsystems
* Receives natural language or structured queries from API
* Calls RBAC module to validate roles
* Forwards NL queries to the NLSQL agent
* Selects correct MCP tool according to parsed action type
* Handles fallback logic, retries, and error responses

**3. Role Validator (RBAC)**

* Resolves user role from database
* Defines permissions for Viewer, Editor, Admin
* Blocks unauthorized operations like DELETE or admin actions
* Sends back pass/fail status to Supervisor Agent

**4. NLSQL Agent**

* Converts natural language into structured intent
* Detects action type (select, insert, update, delete)
* Identifies table, filters, fields, values, ordering, joins
* Normalizes values (dates, numbers, names)
* Produces final structured JSON for MCP tool execution

**5. MCP Client**

* Sends structured operations to MCP server
* Handles authentication headers if used
* Manages network errors
* Normalizes tool response back to Supervisor Agent

**6. MCP Server**

* Hosts executable tools
* Tools required: get_records, create_record, update_record, delete_record
* Optional tools: advanced_query, health_tool
* Performs all DB interactions safely
* Returns structured JSON responses

**7. Database Layer**

* Stores customers, orders, products, employees, and any extended schema
* Must support efficient querying and filtering
* Should have proper indexing for key fields

**8. Audit Log Store**

* Captures user ID, role, action, table, filters, timestamp, MCP tool
* Mandatory for traceability and debugging
* Should store raw NL query and structured query

**SECTION 9: EXTENDED REQUIREMENTS FOR REALISM**
Add these for more real-world exposure:

**1. Pagination Support**
API should support limit, offset, and page parameters.

**2. Server-Side Input Sanitization**
Agent must validate user inputs to prevent:

* invalid types
* SQL injection-like attacks
* dangerous delete-all conditions

**3. Transaction Management**
Bulk operations should run inside DB transactions.

**4. Retry Policies**
If MCP server fails, implement max 3 retries with exponential backoff.

**5. Structured Error Codes**
Example:

* ERR_ROLE_DENIED
* ERR_INVALID_TABLE
* ERR_MCP_TIMEOUT
* ERR_PARSE_FAILURE

**6. MCP Version Switching**
Ability to switch MCP server versions using /mcp/switch endpoint.

**SECTION 10: OPTIONAL ADVANCED FEATURES**
Participants may implement these for bonus points:

* JWT authentication on all endpoints
* IP whitelisting for admin endpoints
* Rate limiting on API routes
* Redis caching for frequently queried data
* Vector-based NL-to-SQL retrieval using embeddings
* Background job queue for heavy operations (Celery / RQ)
* Multi-agent extension where one agent validates, another executes

**SECTION 11: FINAL EXPECTATION**
At the end of Project 180, participants must deliver:

* A fully working agentic backend
* At least eight working API endpoints
* Support for all required scenarios
* Clear enforcement of RBAC
* A functional MCP server hosted locally or remotely
* A complete audit log system
* A readable and structured README
* Postman collection with at least 25 meaningful calls

The end result should feel like a real production-grade microservice that could be used in an enterprise environment.
