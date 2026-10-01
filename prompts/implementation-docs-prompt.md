# Implementation Documentation Task

Based on the implementation you have completed, generate a complete **implementation documentation** for the backend.

The documentation must be split **module by module**, with one Markdown file per module.

## Output Structure

Create the documentation using this structure:

```text
implementation-docs/
├── 1-auth.md
├── 2-users.md
├── 3-<module-name>.md
├── 4-<module-name>.md
└── ...
```

Use the actual modules implemented in the project. Do not group unrelated modules together.

Each module must have its own `.md` file.

---

# Markdown and JSON Formatting Requirements

All generated Markdown must render correctly in standard CommonMark/GitHub-flavoured Markdown viewers.

For every request or response payload:

- Use a fenced code block labelled `json`.
- Include only valid JSON in that block.
- Pretty-print JSON with **two-space indentation**.
- Put each object property and array item on its own appropriate line; never emit minified or single-line JSON.
- Preserve the actual response shape and values. Formatting must not add, remove, or rename fields.

When an example Markdown block contains a nested JSON fence, use four backticks for the outer `md` fence and three backticks for the nested JSON fence. Close each fence with the same number of backticks used to open it.

---

# Important Requirement

**DO NOT SKIP ANY IMPLEMENTED ROUTE, FIELD, VALIDATION, RESPONSE, LOGIC, EXAMPLE, OR IMPORTANT CLIENT INFORMATION.**

The documentation must be generated from the **actual implementation/code**, not assumptions.

If something is not implemented, do not invent it.

If a route has different behaviours depending on the input, role, status, permissions, or other conditions, document those variations.

---

# Required Structure for EVERY Route

Every route documented inside a module must contain sections 1–10 below. Section 11 is conditional: include it only when there is additional important information to report.

## 1. Title

Write the `Title` label and its value on the same heading line.

```md
## Title: <Clear route title>
```

Example:

```md
## Title: Create User
```

---

## 2. Route

Write the `Route` label, HTTP method, and complete API path on the same heading line.

Always include the HTTP method and complete API route.

```md
## Route: POST api/v2/users
```

Use the actual API prefix and route from the implementation.

---

## 3. Authorization

Every route must document authorization immediately after `## Route`, including public routes. Use this exact table structure:

```md
## Authorization

| Authorization           | Roles           | Permissions        |
| ----------------------- | --------------- | ------------------ |
| `Bearer <access-token>` | `HOST`, `ADMIN` | `properties:write` |
```

Rules:

- For public routes, write `No auth` under `Authorization` and `None` under both `Roles` and `Permissions`.
- For routes protected by optional authentication, write `Optional Bearer <access-token>` under `Authorization`. State `None` for roles and permissions unless the implementation applies them when a token is supplied.
- For authenticated routes, state the exact client credential/header required by the implementation, for example `Bearer <access-token>`.
- List every required role under `Roles`; write `None` only when any authenticated user is permitted.
- List every required permission under `Permissions`; write `None` when the implementation checks no explicit permission.
- Do not infer roles, permissions, or authentication schemes from similar routes.

---

## 4. Query Parameters

If the route has query parameters, document **every query parameter in a Markdown table**. Do not describe query parameters in prose or a list.

Use exactly these columns, in this order: `Name`, `Is Optional`, `Default Value`, and `Values`.

```md
## Query

| Name   | Is Optional | Default Value | Values                                      |
| ------ | ----------- | ------------- | ------------------------------------------- |
| page   | Yes         | `1`           | Positive integer                            |
| limit  | Yes         | `20`          | Integer `1`–`100`                           |
| search | Yes         | None          | String; use `john` to search by name/email  |
| role   | Yes         | None          | `USER`, `HOST`, `ADMIN` (only if supported) |
```

Rules:

- Include every supported query parameter as one table row.
- State `Yes` or `No` under `Is Optional`.
- State the actual default under `Default Value`, or `None` when the implementation has no default.
- State the parameter type, validation constraints, accepted values, and a useful example under `Values`.
- For enum, role, status, sort, or other restricted parameters, list every allowed value under `Values`; never use a generic description such as “valid role”.
- Do not invent roles or allowed values. List roles only when the implementation accepts a role query parameter.
- If the route has no query parameters, explicitly write:

```md
## Query

None.
```

Do not omit the section.

---

# 5. Request Body

If the route accepts a request body, document the body.

````md
## Body

```json
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "role": "admin"
}
```
````

The example must represent a **realistic valid payload based on the actual DTO/schema/validation rules**.

Document:

- Required fields
- Optional fields
- Field types
- Allowed values/enums
- Validation rules
- Nested objects
- Arrays
- IDs
- Dates
- Files, if applicable

If there is no request body:

```md
## Body

None.
```

---

# 6. Response

Document the successful response using the actual response structure implemented by the backend.

For every successful response with a JSON body, the `### Response` subsection **must contain a complete, fenced `json` example**. A prose statement such as “the result is returned in `data`” is not a response example and is not allowed.

````md
## Response

### Status Code

201 Created

### Response

```json
{
  "statusCode": 201,
  "data": {
    "user": {
      "id": "66b0c3c2a9561a1e02d0c123",
      "fullName": "Ada Okafor",
      "email": "ada@example.com",
      "phoneNumber": null,
      "profilePicture": null,
      "isEmailVerified": false,
      "roles": []
    },
    "verification": {
      "channel": "EMAIL",
      "expiresInSeconds": 600
    }
  },
  "message": "Request successful",
  "timestamp": "2026-10-01T12:00:00.000Z"
}
```
````

Do not invent response fields.

Use the actual response returned by the implementation.

The example must show the complete API envelope when the global response interceptor applies: `statusCode`, `data`, `message`, and `timestamp`. Include a realistic value for every field returned by the selected execution branch. Do not use `{}`, `...`, `<response data>`, or a partial `data` object as a substitute for the response body. The only exceptions are responses with no body (such as `204 No Content`) and non-JSON redirects/downloads; explicitly state the actual non-JSON behaviour for those exceptions.

Every documented response variant must follow the same rule. For each non-empty JSON error response—including `400 Bad Request`, `401 Unauthorized`, `403 Forbidden`, `404 Not Found`, `409 Conflict`, and `429 Too Many Requests` when implemented—include its status code and a complete, fenced `json` response body. Do not replace an error response body with prose such as “validation errors are returned.” Only state `Empty response body.` when the implementation actually returns no body.

For the global HTTP exception filter, error examples use the implemented shape below; replace the message with the actual route-specific error message:

```json
{
  "statusCode": 404,
  "message": "Property not found",
  "timestamp": "2026-10-01T12:00:00.000Z"
}
```

## Mandatory Response Verification

Before finalizing any module, verify every `### Response` and every documented response-variant code block against the controller, service, response interceptor, and exception filter.

- Verify the HTTP status code against the controller/decorator and actual thrown exception.
- Parse every fenced `json` example as JSON.
- Verify every success example has the exact interceptor envelope and a non-empty, implementation-faithful `data` example when the response has a body.
- Verify every documented 4xx/5xx JSON response has its actual `statusCode`, `message`, and `timestamp` body.
- Verify that no `### Response` or response variant contains prose in place of a required JSON body.
- Verify that `204` and other non-JSON responses are the only documented exceptions.

Do not finish or report the documentation as complete until this verification passes.

---

# 7. Response Variants / Examples

For routes where the response can vary depending on input, state, permissions, resource existence, etc., document the different variants.

For example:

### Example 1 — Successful Request

```json
{
  "statusCode": 201,
  "data": {
    "user": {
      "id": "66b0c3c2a9561a1e02d0c123",
      "fullName": "Ada Okafor",
      "email": "ada@example.com",
      "phoneNumber": null,
      "profilePicture": null,
      "isEmailVerified": false,
      "roles": []
    },
    "verification": {
      "channel": "EMAIL",
      "expiresInSeconds": 600
    }
  },
  "message": "Request successful",
  "timestamp": "2026-10-01T12:00:00.000Z"
}
```

### Example 2 — Resource Not Found

```json
{
  "statusCode": 404,
  "message": "User not found",
  "timestamp": "2026-10-01T12:00:00.000Z"
}
```

### Example 3 — Validation Error

```json
{
  "statusCode": 400,
  "message": "email must be an email",
  "timestamp": "2026-10-01T12:00:00.000Z"
}
```

Include the correct HTTP status code for each response.

Do this whenever the implementation has meaningful response variations.

---

# 8. Additional Request Examples

If a route supports different valid payloads or important use cases, provide additional examples.

For example:

### Example 1 — Admin User

```json
{
  "role": "admin",
  "email": "admin@example.com"
}
```

### Example 2 — Regular User

```json
{
  "role": "user",
  "email": "user@example.com"
}
```

Only include examples supported by the actual implementation.

Do not create hypothetical payloads.

---

# 9. Client Note

Every route must have a client implementation guide.

```md
## Client Note

- The client must send the authentication token in the Authorization header.
- The client should send the request as `application/json`.
- The `role` field accepts only the documented role values.
- The client should handle `404` when the requested resource does not exist.
- After a successful request, the client can use the returned `data.id`.
```

Explain what the frontend/mobile/client developer needs to know to correctly consume the endpoint.

Include things such as:

- Authentication requirements
- Required headers
- Content-Type
- Pagination behaviour
- Sorting
- Filtering
- File uploads
- Enum values
- Date formats
- ID formats
- Token requirements
- Permission requirements
- Special client-side handling
- Expected UI behaviour when appropriate
- How to handle important response states

Do not document backend internals here unless they directly affect the client.

---

# 10. Backend Logic

Every route must contain a backend implementation/logic section.

```md
## Logic

1. Authenticate the request.
2. Validate the request payload.
3. Check whether the authenticated user has permission.
4. Check whether the requested resource exists.
5. Perform the required database operation.
6. Return the appropriate response.
```

This section must explain what the backend actually does.

Include:

- Authentication checks
- Authorization checks
- Role/permission checks
- Validation
- Database queries
- Existence checks
- Duplicate checks
- Business rules
- Status checks
- Relationship checks
- Tenant/organization checks
- Transactions
- External API calls
- Side effects
- Notifications
- Events
- File operations
- Error handling
- Any other important backend behaviour

Document the **actual order and conditions of the logic** where relevant.

---

# 11. Important Information

Include this section only when the route has important client-facing information that does not fit into the sections above.

```md
## Important Information

- This endpoint requires authentication.
- The resource belongs to the authenticated user's organization.
- Updating this resource may trigger a notification.
- The operation cannot be performed when the resource is archived.
```

If there is nothing additional, omit the `## Important Information` section entirely. Do not write `None.` for this section.

---

# Required Route Documentation Template

Every route must follow this complete structure:

````md
# <Module Name>

## Title: <Create route title>

## Route: <METHOD> api/v2/<route>

## Authorization

| Authorization                                                             | Roles                  | Permissions                  |
| ------------------------------------------------------------------------- | ---------------------- | ---------------------------- |
| <`Bearer <access-token>`, `Optional Bearer <access-token>`, or `No auth`> | <Exact roles, or None> | <Exact permissions, or None> |

## Query

| Name | Is Optional | Default Value | Values |
| ---- | ----------- | ------------- | ------ |
| ...  | ...         | ...           | ...    |

## Body

```json
{
  "exampleField": "example value"
}
```

## Response

### Status Code

<HTTP status code> <status text>

### Response

```json
{
  "statusCode": 200,
  "data": {
    "id": "66b0c3c2a9561a1e02d0c123"
  },
  "message": "Request successful",
  "timestamp": "2026-10-01T12:00:00.000Z"
}
```

### Response Variants

#### Example 1 — <Description>

**Status Code:** <HTTP status>

```json
{
  "statusCode": 400,
  "message": "<actual validation error message>",
  "timestamp": "2026-10-01T12:00:00.000Z"
}
```

#### Example 2 — <Description>

**Status Code:** <HTTP status>

```json
{
  "statusCode": 404,
  "message": "<actual not-found error message>",
  "timestamp": "2026-10-01T12:00:00.000Z"
}
```

## Additional Examples

### Example 1 — <Description>

```json
{
  "exampleField": "example value"
}
```

### Example 2 — <Description>

```json
{
  "exampleField": "example value"
}
```

## Client Note

<Everything the client/frontend/mobile developer needs to know>

## Logic

<Actual backend implementation logic>

<!-- Omit this entire optional block when there is no additional important information. -->

## Important Information

<Additional important information>
````

If a required section does not apply, **DO NOT REMOVE IT**. Write `None.` instead. The only exception is `## Important Information`, which must be omitted when there is nothing important to report.

---

# Module-Level Requirements

At the beginning of every module document, include:

```md
# <Module Name>

## Overview

<What this module is responsible for>

## Routes

| Method | Route      | Description |
| ------ | ---------- | ----------- |
| POST   | api/v2/... | ...         |
| GET    | api/v2/... | ...         |
| PATCH  | api/v2/... | ...         |
| DELETE | api/v2/... | ...         |
```

Then document **every route in that module** using the required route structure.

---

# Accuracy Requirements

The documentation must be based on the actual implementation.

Before generating the documentation, inspect the relevant:

- Controllers
- Routes
- DTOs
- Schemas/models
- Services
- Guards
- Middleware
- Pipes
- Validators
- Decorators
- Enums
- Authentication logic
- Authorization logic
- Database queries
- External service integrations
- Response serializers
- Error handling
- Related modules

Trace each route from:

```text
Request
  ↓
Controller
  ↓
Guards / Middleware / Pipes
  ↓
Validation
  ↓
Service
  ↓
Database / External Service
  ↓
Response
```

Use this trace to document the actual implementation.

---

# Do Not Make Assumptions

Do not:

- Invent routes.
- Invent request fields.
- Invent response fields.
- Invent validation rules.
- Invent status codes.
- Invent permissions.
- Invent database behaviour.
- Invent error responses.
- Invent business rules.
- Assume a route behaves like another route.
- Replace actual implementation details with generic explanations.

If the implementation does something unusual, document the unusual behaviour exactly.

---

# Completeness Check

Before finishing each module, verify:

- [ ] Every implemented route is documented.
- [ ] Every route has a Title.
- [ ] Every route has a Route.
- [ ] Every route has an Authorization section immediately after Route.
- [ ] Every Authorization section states the exact authorization method, all required roles, and all required permissions.
- [ ] Every route has a Query section.
- [ ] Every route has a Body section.
- [ ] Every route has a Response section.
- [ ] Every relevant response variant is documented.
- [ ] Every relevant payload variant is documented.
- [ ] Every route has a Client Note.
- [ ] Every route has a Logic section.
- [ ] Every route with additional important information has an Important Information section.
- [ ] Routes with no additional important information omit that section instead of writing `None.`.
- [ ] HTTP methods are correct.
- [ ] API paths are correct.
- [ ] Query parameters are complete.
- [ ] Every route with query parameters uses the exact `Name | Is Optional | Default Value | Values` table format.
- [ ] Every enum, role, status, or other restricted query parameter lists its actual allowed values in the `Values` column.
- [ ] Request body examples match the actual validation rules.
- [ ] Response examples match the actual implementation.
- [ ] Every JSON success response has a complete, fenced, pretty-printed JSON example; no JSON response is described only in prose.
- [ ] Every documented non-empty error response, including `404` where implemented, has a complete fenced JSON body; no error response is described only in prose.
- [ ] Every response example has been parsed as JSON and verified against the controller, service, response interceptor, and exception filter.
- [ ] No response example uses `{}`, `...`, a placeholder, or a partial `data` object in place of the actual returned body.
- [ ] Status codes match the implementation.
- [ ] Authentication requirements are documented.
- [ ] Authorization/permission requirements are documented.
- [ ] Important edge cases are documented.
- [ ] No implementation details were invented.

**None of these steps should be skipped.**

The final output should be the complete `implementation-docs/` directory with one Markdown document per module.
