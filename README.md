# PROJECT 180

## Real-World Agentic AI Backend Challenge

NLSQL Agent • MCP Server • Role-Based Access • REST API System


### 1. Introduction

Project 180 is a real-world backend AI engineering challenge designed to help participants build a production-style agentic system.
The goal is to create a backend service where users can interact with a database using natural language. The system converts natural language into structured operations, validates user permissions, calls MCP tools, and executes safe CRUD operations.

This project is backend-only. All interaction happens through REST APIs and Postman.

Participants must design an NLSQL Agent pipeline, an MCP server with database tools, a role-based access system, and a complete audit trail—similar to how enterprise AI features are built in production.

### 2. Problem Statement

Build a backend system that enables the following flow:

1. User sends a natural language query

2. Backend forwards it to an Agent

3. Agent parses the intent using NLSQL

4. Role validator checks user permissions

5. Agent selects the correct MCP tool

6. MCP server executes the DB operation

7. Response is returned as structured JSON

8. Action is logged in the audit system

9. Participants must implement at least eight (8) REST API endpoints and support multiple real-world scenarios such as filtering, joins, bulk operations, and admin-only actions.

# Architecture

![System Architecture](./architecture.png)

## Example queries the system must be able to interpret:

“Show all customers from Dubai”

“Update price of product 10 to 1800”

“Add a new employee named Sara from HR”

“Delete order 17”

“Show customers who joined this month”

“List orders with their customer names”

All these must be parsed into structured operations (action, table, conditions, values, filters, limits, etc.)

# 3. Role-Based Access Control (RBAC)

The system must support the following roles:

**1.Viewer** Read-only, Can run SELECT-type queries only

**2.Editor** Can read, create, and update but Cannot delete

**3.Admin** Full access including DELETE, Can run reset and seed operations, Can switch MCP server URL

## All permission checks must happen inside the agent pipeline, not just at the API level.

# 4. Required API Endpoints (Minimum 8)

Participants must implement at least the following:

1. POST /query
Accepts natural language and triggers the agent flow.

2. POST /query/structured
Accepts structured JSON for direct execution (debugging).

3. POST /register
Register a new user with a role.

4. GET /users
List all users.

5. GET /audit-log
Return full audit trail.

6. GET /mcp/health
Check MCP server availability and tool readiness.

7. POST /admin/seed
Seed initial dataset (Admin only).

8. POST /admin/reset
Reset/clear the database entirely (Admin only).

9. Additional recommended endpoints (optional but valuable):

GET /roles

GET /db/schema

GET /audit-log/user/{id}

POST /mcp/switch

# 5. Required Scenarios

The following real-world scenarios must be supported:

Scenario 1: Basic Retrieval

“Show all customers from Dubai.”

Scenario 2: Create Operation

“Add a new product called MacBook Air priced at 4500.”

Scenario 3: Update Operation

“Update salary of employee 9 to 15000.”

Scenario 4: Delete Operation (Admin only)

“Delete customer 11.”

Scenario 5: Complex Filtering

“Show all orders placed this month with total above 500.”

Scenario 6: Sorting & Pagination

“Show latest 5 customers sorted by date descending.”

Scenario 7: Join Interpretation

“Show all orders with customer names.”

Scenario 8: Bulk Insert

“Add employees John, Sara, and Alex to Operations.”

Scenario 9: Bulk Update

“Increase all electronics product prices by 10 percent.”

Scenario 10: Error Handling

Table doesn’t exist, invalid field, insufficient role, etc.