# Implementation Documentation Task

Based on the backend implementation that has been completed, generate a complete, polished, developer-friendly **implementation documentation suite**.

The documentation must be based entirely on the **actual implementation/codebase**.

The final documentation should be:

- Accurate enough for backend engineers.
- Easy to scan for frontend/mobile engineers.
- Visually polished in GitHub-flavoured Markdown.
- Consistent across all modules.
- Complete enough that another developer can integrate with the backend without reading the source code for normal API usage.
- Structured like professional API documentation rather than raw engineering notes.

---

# 1. Primary Objective

Generate implementation documentation for every implemented backend module.

The documentation must be split **module by module**, with one Markdown file per module.

Example:

```text
implementation-docs/
├── README.md
├── 1-auth.md
├── 2-users.md
├── 3-properties.md
├── 4-bookings.md
├── 5-payments.md
└── ...
```

Use the actual modules implemented in the project.

Do not:

- Group unrelated modules together.
- Invent modules.
- Skip implemented modules.
- Combine endpoints simply to reduce documentation size.

Each module must have its own `.md` file.

Also generate:

```text
implementation-docs/README.md
```

The README acts as the entry point and navigation page for the complete documentation.

---

# 2. Documentation Design Principles

The documentation must prioritize both:

1. **Implementation accuracy**
2. **Excellent readability**

Do not produce visually flat documentation where every piece of information has the same hierarchy.

Use:

- Strong heading hierarchy.
- Endpoint numbering.
- Summary tables.
- Short explanatory text.
- Clear field tables.
- Proper spacing.
- Consistent formatting.
- Meaningful callouts.
- Complete JSON examples.
- Clear separation between endpoints.

Avoid:

- Huge unbroken paragraphs.
- Excessive prose.
- Repeating the route in several different headings.
- Generic explanations.
- Decorative content that adds no technical value.
- Excessive nesting.
- HTML-heavy layouts.
- Emojis as structural elements.
- Fake badges or images.
- One-line/minified JSON.

The Markdown must remain clean and readable in standard GitHub-flavoured Markdown viewers.

---

# 3. Global Documentation README

Create:

```text
implementation-docs/README.md
```

Use this layout:

```md
# Backend Implementation Documentation

## Overview

<Short description of the backend and what this documentation covers.>

## API Conventions

| Item | Value |
| --- | --- |
| Base API prefix | `/api/v2` |
| Authentication | `Bearer <access-token>` |
| Request format | `application/json` |
| Response format | `application/json` |
| ID format | <actual implementation> |
| Date format | <actual implementation> |

Only include conventions verified from the implementation.

## Modules

| # | Module | Description | Endpoints |
| --- | --- | --- | ---: |
| 1 | [Authentication](./1-auth.md) | Account authentication and session management | 10 |
| 2 | [Users](./2-users.md) | User management | 8 |
| ... | ... | ... | ... |

## Global Response Format

<Document the global success envelope if one exists.>

```json
{
  "statusCode": 200,
  "data": {},
  "message": "Request successful",
  "timestamp": "2026-10-01T12:00:00.000Z"
}
```

## Global Error Format

<Document the global error envelope if one exists.>

```json
{
  "statusCode": 400,
  "message": "Example error",
  "timestamp": "2026-10-01T12:00:00.000Z"
}
```

## Authentication

<Brief explanation based on the actual guards/authentication implementation.>

## Documentation Index

- [Authentication](./1-auth.md)
- [Users](./2-users.md)
- ...
```

Do not invent global conventions.

If a convention varies between modules or routes, say so.

---

# 4. Module Document Layout

Every module must use the following top-level structure.

```md
# Authentication

> Authentication, account verification, password recovery, session management, and OAuth functionality.

## Overview

<Concise explanation of the module's responsibility.>

## Endpoint Index

| # | Method | Endpoint | Purpose | Auth |
| ---: | --- | --- | --- | --- |
| 1 | `POST` | `/api/v2/auth/signup` | Create an account | Public |
| 2 | `POST` | `/api/v2/auth/signin` | Sign in | Public |
| 3 | `POST` | `/api/v2/auth/logout` | Revoke a session | Bearer |
| ... | ... | ... | ... | ... |

## Module Notes

Only include this section when there are important behaviours that apply to several endpoints.

Examples:

- Global throttling behaviour.
- Shared authentication guards.
- Shared DTO conventions.
- Shared serialization behaviour.
- Shared tenant restrictions.
- Shared response interceptors.

Do not repeat information here if it applies to only one endpoint.

---

# Endpoints

## 1. Sign up

`POST /api/v2/auth/signup`

> Create a new user account and begin email verification.

...

---

## 2. Sign in

`POST /api/v2/auth/signin`

> Authenticate a user and issue an access/refresh-token pair.

...
```

Use a horizontal rule:

```md
---
```

between routes.

This visual separation is mandatory.

---

# 5. Endpoint Heading Design

Do NOT use this pattern:

```md
## Title: Sign up

## Route: POST /api/v2/auth/signup
```

Instead, each endpoint must begin like this:

```md
## 1. Sign up

`POST /api/v2/auth/signup`

> Create a new account and begin email verification.
```

Rules:

- Number endpoints according to their order in the module's Endpoint Index.
- Use a clear human-readable endpoint name.
- Put the method and complete route immediately beneath the title.
- Add a one-sentence description beneath the route.
- Use the actual API prefix.
- Do not add descriptions that are not supported by implementation behaviour.

---

# 6. Endpoint Overview

Immediately after the route introduction, include:

```md
### Endpoint Overview

| Property | Value |
| --- | --- |
| Authentication | No auth |
| Roles | None |
| Permissions | None |
| Content-Type | `application/json` |
| Success Status | `201 Created` |
| Rate Limit | 5 requests / 60 seconds |
```

Only include rows relevant to the route.

Supported rows include:

- Authentication
- Roles
- Permissions
- Content-Type
- Success Status
- Rate Limit
- Response Type
- Upload Type
- Cache Behaviour
- Idempotency
- Other important request-level metadata

Rules:

- `Authentication` must always appear.
- `Roles` must always appear.
- `Permissions` must always appear.
- `Success Status` must always appear.
- Do not invent metadata.
- Omit optional rows that do not apply.

Authentication values should be implementation-faithful, for example:

```text
No auth
Bearer <access-token>
Optional Bearer <access-token>
API key
```

For public routes:

```text
Authentication: No auth
Roles: None
Permissions: None
```

For authenticated routes without role restrictions:

```text
Authentication: Bearer <access-token>
Roles: None
Permissions: None
```

List every actual role and permission where restrictions exist.

---

# 7. Request

Every endpoint must contain:

```md
### Request
```

Under Request, use the appropriate subsections.

Recommended order:

```md
#### Query Parameters
#### Path Parameters
#### Headers
#### Body
#### Body Fields
#### Files
```

Only `Query Parameters` and `Body` are mandatory sections.

Other sections should be included when applicable.

---

# 8. Query Parameters

Every endpoint must contain:

```md
#### Query Parameters
```

If there are no query parameters:

```md
#### Query Parameters

None.
```

If query parameters exist, use exactly:

```md
#### Query Parameters

| Name | Type | Required | Default | Accepted Values / Validation | Example |
| --- | --- | --- | --- | --- | --- |
| `page` | integer | No | `1` | Positive integer | `2` |
| `limit` | integer | No | `20` | `1`–`100` | `50` |
| `search` | string | No | None | Free-text search | `john` |
| `status` | string | No | None | `ACTIVE`, `INACTIVE` | `ACTIVE` |
```

Rules:

- Document every supported parameter.
- State the actual type.
- Use `Yes` or `No` for Required.
- Show actual defaults.
- List every enum/restricted value.
- Include actual validation constraints.
- Do not say only “valid status”.
- Do not invent allowed values.

---

# 9. Path Parameters

When a route contains path parameters, include:

```md
#### Path Parameters

| Name | Type | Required | Validation | Example |
| --- | --- | --- | --- | --- |
| `id` | string | Yes | Valid MongoDB ObjectId | `66b0c3c2a9561a1e02d0c123` |
```

Only document validation that actually exists or is enforced by implementation behaviour.

If there are no path parameters, omit this subsection.

---

# 10. Headers

Include a Headers subsection only when headers require explanation beyond what is already obvious from Endpoint Overview.

Example:

```md
#### Headers

| Header | Required | Value |
| --- | --- | --- |
| `Authorization` | Yes | `Bearer <access-token>` |
| `Content-Type` | Yes | `application/json` |
```

For file uploads:

```md
| `Content-Type` | Yes | `multipart/form-data` |
```

Do not invent headers.

---

# 11. Request Body

Every endpoint must contain:

```md
#### Body
```

If the endpoint has no request body:

```md
#### Body

None.
```

If it accepts JSON, include a complete realistic payload:

```json
{
  "fullName": "Ada Okafor",
  "email": "ada@example.com",
  "password": "SecurePass8",
  "recaptchaToken": "client-recaptcha-token"
}
```

The example must:

- Be valid according to the actual DTO/schema.
- Use realistic values.
- Include every required field.
- Include useful optional fields where appropriate.
- Never include fields rejected/stripped by the implementation unless the purpose is specifically to document that behaviour.

---

# 12. Body Field Documentation

For every non-trivial request body, immediately follow the JSON example with:

```md
#### Body Fields

| Field | Type | Required | Validation / Accepted Values | Notes |
| --- | --- | --- | --- | --- |
| `fullName` | string | Yes | <actual validation> | User's display name |
| `email` | string | Yes | Valid email address | Normalized to lowercase |
| `password` | string | Yes | 8–255 characters | Never returned by the API |
| `recaptchaToken` | string | Yes | Non-empty string | Verified server-side |
| `referralCode` | string | No | <actual validation> | <implementation behaviour> |
```

Rules:

- Include every accepted body field.
- State nested fields.
- State arrays.
- State enum values.
- State nullable fields.
- State required/optional status.
- State actual validation.
- Document normalization such as trimming/lower-casing when confirmed.
- Mention ignored/accepted-but-unused fields when this matters.
- Do not invent descriptions from field names alone.

For nested objects, use dot notation where useful:

```text
address.street
address.city
items[].productId
```

---

## 12A. Type-Dependent and Discriminated Bodies

When a request body changes shape based on a discriminator such as `type`, `kind`, `mode`, `action`, `provider`, or `status`, document the full contract for **every supported value**. This also applies to DTOs that use conditional validation (for example, `ValidateIf`) or union/`oneOf` schemas.

Do not document one representative payload and imply that the remaining variants are similar.

For each discriminator, include:

- The exact accepted value, taken from the implementation.
- The fields required for that value.
- The optional fields for that value.
- Fields that are invalid, ignored, or not applicable for that value.
- Nested fields, file fields, enum values, limits, and validation rules that change by type.
- Whether the discriminator itself may be changed by an update endpoint.
- Any type-specific response fields or behaviour.

Add a matrix before the examples when there are two or more variants:

```md
| `type` value | Required fields | Optional fields | Not applicable / rejected | Notes |
| --- | --- | --- | --- | --- |
| `MEAL` | `type`, `meal` | `note` | `napTimes`, `attachments` | <actual behaviour> |
| `PHOTO` | `type`, `attachments` | `description` | `meal`, `napTimes` | <actual behaviour> |
```

Then provide one complete, independently valid JSON request example for every supported value. The examples must be sufficient for a client to implement each variant without inspecting backend source code.

If the API accepts multipart data for only some variants, document that distinction in both the matrix and the Files section. Do not infer type-specific rules from field names; verify them in the DTO, controller, service, and schema.

---

# 13. File Uploads

For multipart/file routes, document files separately.

Example:

```md
#### Files

| Field | Required | Type | Accepted Formats | Limits |
| --- | --- | --- | --- | --- |
| `profilePicture` | Yes | File | `image/jpeg`, `image/png` | Maximum 5 MB |
```

Document only actual limits and file types.

---

# 14. Success Response

Every endpoint must contain:

```md
### Success Response
```

Then:

```md
**Status:** `201 Created`
```

Follow it immediately with a complete response example.

Example:

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

Rules:

- Show the full response.
- Include the global envelope when the interceptor applies.
- Include realistic values.
- Include all returned fields for the selected branch.
- Preserve nested object shapes.
- Preserve arrays.
- Preserve nullability.
- Do not use placeholders such as:

```text
...
{}
<data>
<response>
```

unless the implementation literally returns such a value.

For `204 No Content`:

```md
### Success Response

**Status:** `204 No Content`

Empty response body.
```

For redirects:

```md
### Success Response

**Status:** `302 Found`

The request is redirected to <actual destination/flow>.
No JSON response body is returned.
```

Do not fabricate a JSON envelope for redirects, downloads, streams, or empty responses.

---

# 15. Response Variants

Every endpoint must contain:

```md
### Response Variants
```

If no meaningful implementation-specific variants exist:

```md
### Response Variants

None.
```

Otherwise first provide a compact overview:

```md
### Response Variants

| Status | Condition |
| --- | --- |
| `400 Bad Request` | Validation or invalid OTP |
| `401 Unauthorized` | Invalid authentication credentials |
| `409 Conflict` | Account already exists |
| `429 Too Many Requests` | Rate limit exceeded |
```

Then document each important variant.

Example:

```md
#### `400 Bad Request` — Invalid OTP

```json
{
  "statusCode": 400,
  "message": "Invalid or expired OTP.",
  "timestamp": "2026-10-01T12:00:00.000Z"
}
```

#### `401 Unauthorized` — Invalid credentials

```json
{
  "statusCode": 401,
  "message": "Invalid credentials.",
  "timestamp": "2026-10-01T12:00:00.000Z"
}
```
```

Rules:

- Include actual status codes.
- Include actual messages where determinable.
- Do not collapse multiple materially different behaviours into a vague generic error.
- Do not invent error branches.
- When the exact error is delegated to an external/internal service and cannot be statically determined, explain that accurately rather than inventing a message.
- Every JSON error example must contain the actual global error shape.

---

# 16. Additional Request Examples

Every endpoint must contain:

```md
### Additional Request Examples
```

If none are useful:

```md
### Additional Request Examples

None.
```

Add examples only when the implementation supports meaningfully different valid request patterns.

For a type-dependent or discriminated request body, examples for every supported discriminator value are mandatory, even if the values seem similar. Place the complete per-type examples in the endpoint's request-body section or in this section, but never omit a supported variant.

Examples include:

- Different supported enum modes.
- Optional fields.
- Different authentication paths.
- Different resource types.
- Different filter combinations.
- Different payment methods.
- Different role-specific input.

Do not add examples merely to fill space.

Example:

```md
#### Minimal Payload

```json
{
  "email": "user@example.com",
  "password": "SecurePass8"
}
```

#### Payload with Optional Referral Code

```json
{
  "email": "user@example.com",
  "password": "SecurePass8",
  "referralCode": "ADA123"
}
```
```

Only include combinations supported by implementation.

---

# 17. Client Integration Notes

Every route must contain:

```md
### Client Integration
```

Make this section practical and frontend-oriented.

Prefer concise bullets.

Example:

```md
### Client Integration

- Send the payload as `application/json`.
- No bearer token is required.
- After successful signup, transition the user to the email-verification flow.
- Treat the returned user `id` as an opaque identifier.
- The verification code expires after 600 seconds.
- Handle `429` by disabling immediate retry and displaying an appropriate retry message.
```

Explain only what affects clients.

Possible topics:

- Authentication requirements.
- Required headers.
- Token storage/use.
- Content type.
- Pagination.
- Sorting.
- Filtering.
- File uploads.
- Enum values.
- Date formats.
- ID handling.
- Redirect handling.
- Permission-specific UI states.
- Retry behaviour.
- Important status codes.
- Follow-up screens/actions.
- Polling.
- Disabled states.
- Upload progress.
- Session invalidation.

Do not dump backend implementation internals into this section.

---

# 18. Backend Behaviour

Every route must contain:

```md
### Backend Behaviour
```

Use a numbered flow.

Example:

```md
### Backend Behaviour

1. Applies the route's public rate limiter.
2. Validates and transforms the DTO.
3. Verifies the supplied reCAPTCHA token using the client IP.
4. Checks whether an account with the supplied email already exists.
5. Creates the user.
6. Generates the email-verification OTP.
7. Sends the verification message.
8. Reloads the safe account projection.
9. Returns the response through the global response interceptor.
```

Requirements:

- Follow the actual execution order where relevant.
- Mention conditions/branches.
- Mention guards.
- Mention middleware.
- Mention validation.
- Mention permission checks.
- Mention tenant checks.
- Mention resource lookups.
- Mention duplicate detection.
- Mention database operations.
- Mention transactions.
- Mention external services.
- Mention file operations.
- Mention notifications.
- Mention events.
- Mention session creation/revocation.
- Mention side effects.
- Mention important error conditions.

Do not reduce every endpoint to generic text such as:

```text
1. Validate request.
2. Call service.
3. Return response.
```

The section must explain the actual implementation.

---

# 19. Important Behaviour

Include:

```md
### Important Behaviour
```

only when there is unusual or important information not already obvious elsewhere.

Do NOT include this section simply because the template contains it.

Good examples:

```md
### Important Behaviour

> The submitted `referralCode` is accepted by the DTO but is not currently forwarded to the signup service.
```

or:

```md
### Important Behaviour

> Password reset revokes all active refresh sessions for the account.
```

or:

```md
### Important Behaviour

> The endpoint intentionally returns the same success response whether or not the email exists, preventing account enumeration.
```

Use this section for:

- Security-sensitive behaviour.
- Unexpected implementation behaviour.
- Accepted-but-unused values.
- Irreversible actions.
- Cross-resource side effects.
- Compatibility concerns.
- Important current implementation limitations.

Do not use it for generic reminders.

---

# 20. Complete Endpoint Template

Use this pattern for every route:

````md
## <number>. <Endpoint Name>

`<METHOD> /api/v2/<route>`

> <One-sentence purpose.>

### Endpoint Overview

| Property | Value |
| --- | --- |
| Authentication | <actual value> |
| Roles | <actual roles or None> |
| Permissions | <actual permissions or None> |
| Success Status | `<status>` |
| Content-Type | `<type>` |

### Request

#### Query Parameters

<Actual table or None.>

#### Path Parameters

<Only when applicable.>

#### Headers

<Only when useful/applicable.>

#### Body

```json
{
  "example": "value"
}
```

#### Body Fields

| Field | Type | Required | Validation / Accepted Values | Notes |
| --- | --- | --- | --- | --- |
| `example` | string | Yes | <actual validation> | <actual behaviour> |

### Success Response

**Status:** `200 OK`

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

| Status | Condition |
| --- | --- |
| `400 Bad Request` | <actual condition> |
| `404 Not Found` | <actual condition> |

#### `400 Bad Request` — <Description>

```json
{
  "statusCode": 400,
  "message": "<actual error>",
  "timestamp": "2026-10-01T12:00:00.000Z"
}
```

### Additional Request Examples

<Actual examples or None.>

### Client Integration

- <Relevant integration information>
- <Relevant integration information>

### Backend Behaviour

1. <Actual step>
2. <Actual step>
3. <Actual step>

### Important Behaviour

> <Only include when genuinely necessary.>

---
````

Rules:

- `Important Behaviour` is optional.
- `Path Parameters` is optional.
- `Headers` is optional.
- `Body Fields` can be omitted only when there is no body.
- Every other major section shown above is required.
- If a required section has no applicable content, write `None.`.

---

# 21. Markdown and JSON Formatting Requirements

All generated Markdown must render correctly in standard CommonMark/GitHub-flavoured Markdown viewers.

For every JSON request or response:

- Use a fenced code block labelled `json`.
- Include valid JSON only.
- Use two-space indentation.
- Put object properties on separate lines.
- Put array items on appropriate separate lines.
- Never output minified JSON.
- Never include comments inside JSON.
- Never use JavaScript syntax.
- Never use trailing commas.
- Never place prose inside the JSON fence.

When an example Markdown block contains a nested JSON fence, use four backticks for the outer `md` block and three backticks for the JSON block.

---

# 22. Documentation Accuracy Requirements

**DO NOT SKIP ANY IMPLEMENTED ROUTE, FIELD, VALIDATION, RESPONSE, LOGIC, EXAMPLE, OR IMPORTANT CLIENT INFORMATION.**

Documentation must come from actual implementation.

Do not document what the API *should* do.

Document what it **currently does**.

If the implementation appears unusual or potentially incorrect, document the current behaviour rather than silently correcting it.

For example:

> The controller currently returns `201 Created` because it does not override the framework's default POST status.

This is preferable to changing the documented result to `200 OK`.

---

# 23. Required Code Inspection

Before documenting a module, inspect all relevant implementation layers.

At minimum inspect, where applicable:

- Controllers.
- Route definitions.
- DTOs.
- Request schemas.
- Response DTOs.
- Database schemas/models.
- Services.
- Guards.
- Middleware.
- Pipes.
- Interceptors.
- Validators.
- Decorators.
- Enums.
- Authentication logic.
- Authorization logic.
- Permission checks.
- Role checks.
- Database operations.
- Repository methods.
- Transactions.
- External integrations.
- Response serializers.
- Exception filters.
- File-storage services.
- Queues/jobs.
- Event emitters/listeners.
- Related modules.

Do not document a controller in isolation when behaviour depends on another layer.

---

# 24. Trace Every Endpoint

For every route, trace:

```text
Incoming Request
      ↓
Controller / Route Handler
      ↓
Guards
      ↓
Middleware
      ↓
Pipes / DTO Validation
      ↓
Authorization / Permission Checks
      ↓
Service
      ↓
Repository / Database
      ↓
External Services / Side Effects
      ↓
Serializer / Interceptor
      ↓
Exception Filter
      ↓
Final HTTP Response
```

Not every project uses every layer.

Skip nonexistent layers rather than inventing them.

Use this trace to determine:

- Accepted inputs.
- Transformations.
- Authorization.
- Business rules.
- Database effects.
- Side effects.
- Errors.
- Final response shape.

---

# 25. Build an Internal Endpoint Inventory First

Before writing a module document, internally create an endpoint inventory.

For every discovered endpoint determine:

- Module.
- Controller.
- HTTP method.
- Full route.
- Handler.
- Authentication.
- Roles.
- Permissions.
- Query parameters.
- Path parameters.
- Body DTO.
- File inputs.
- Success status.
- Success return value.
- Error branches.
- Service calls.
- Database calls.
- Side effects.

Compare the final documentation against this inventory.

Do not expose this scratch inventory unless explicitly requested.

Its purpose is to ensure no endpoint is omitted.

---

# 26. Do Not Make Assumptions

Do not:

- Invent routes.
- Invent fields.
- Invent DTO properties.
- Invent query parameters.
- Invent enum values.
- Invent responses.
- Invent response envelopes.
- Invent HTTP status codes.
- Invent validations.
- Invent roles.
- Invent permissions.
- Invent authentication requirements.
- Invent business rules.
- Invent database behaviour.
- Invent external-service behaviour.
- Invent notifications.
- Invent errors.
- Infer one endpoint's behaviour from another endpoint.
- Use generic conventional REST behaviour when the code behaves differently.

If implementation evidence is insufficient, explicitly state the limitation.

Example:

```md
> The controller delegates this error to the payment provider. The exact provider error message is not statically defined in this codebase.
```

Do not replace uncertainty with invention.

---

# 27. Response Verification

Before finalizing an endpoint, verify the response against all relevant layers:

- Controller return value.
- Controller HTTP status.
- Service result.
- Serializer.
- Global response interceptor.
- Exception filter.

For success responses verify:

- Correct status code.
- Correct global envelope.
- Correct `data` shape.
- Correct nested fields.
- Correct nullability.
- Correct arrays.
- Correct message behaviour.

For error responses verify:

- Actual status code.
- Actual message or message source.
- Actual error envelope.
- Whether validation errors use the same shape.
- Whether external-provider errors are transformed.

Do not document a response until this verification is complete.

---

# 28. JSON Validation

Every generated fenced `json` block must be valid JSON.

Before completing the documentation:

- Parse every generated JSON example.
- Fix malformed blocks.
- Ensure braces match.
- Ensure brackets match.
- Ensure strings use double quotes.
- Remove trailing commas.
- Ensure nested objects are properly closed.
- Ensure examples match the implementation.

No documentation file should be considered complete while it contains invalid JSON.

---

# 29. Response Examples Must Be Complete

For JSON success responses, never write:

```json
{}
```

Never write:

```json
{
  "data": "..."
}
```

Never write:

```json
{
  "data": {}
}
```

when the implementation returns a populated object.

Never write:

```text
Returns the created user.
```

instead of the response body.

Show a complete representative example.

The only exceptions are:

- `204 No Content`.
- Redirects.
- Streams.
- Downloads.
- Other genuinely non-JSON responses.

Describe those behaviours explicitly.

---

# 30. Security-Sensitive Documentation

Document relevant security behaviour without exposing secrets.

Include actual client-visible behaviour such as:

- Bearer token requirements.
- Refresh-token handling.
- OTP expiry.
- Session revocation.
- Role restrictions.
- Permissions.
- Rate limits.
- Account-enumeration protections.
- Token purpose validation.
- File restrictions.
- Tenant isolation.

Never expose:

- Secret keys.
- Environment secret values.
- Private signing keys.
- Database credentials.
- Provider credentials.

Use placeholders where a secret value is naturally required:

```text
<access-token>
<refresh-token>
<google-client-secret>
```

---

# 31. Writing Style

Write like high-quality internal developer documentation.

Use:

- Direct language.
- Short paragraphs.
- Tables for structured facts.
- Bullets for client guidance.
- Numbered lists for backend execution flows.
- Inline code formatting for fields, routes, enums, headers, methods, IDs, and technical values.

Prefer:

```md
The client must send `Authorization: Bearer <access-token>`.
```

instead of:

```md
The frontend developer should make sure that whenever they want to use this endpoint, they include the access token in the authorization header.
```

Keep prose concise without removing technical details.

---

# 32. Avoid Visual Noise

Do not make every sentence a heading.

Do not use excessive levels such as:

```text
#####
######
```

without need.

The preferred hierarchy is:

```text
# Module
## Endpoint
### Major endpoint section
#### Endpoint subsection
```

Avoid going deeper unless absolutely required.

Do not use repeated labels such as:

```text
## Title
## Route
## Authorization
## Query
## Body
```

for every endpoint.

The route's name itself is the endpoint heading.

---

# 33. Tables

Keep tables readable.

Use tables for:

- Endpoint indexes.
- Endpoint metadata.
- Query parameters.
- Path parameters.
- Body fields.
- File fields.
- Response status summaries.

Do not put large JSON objects inside tables.

Do not create extremely wide tables when information is better placed in Notes.

Use inline code formatting around:

- Field names.
- Enum values.
- HTTP methods.
- Routes.
- IDs.
- Statuses when appropriate.
- Header names.

---

# 34. Client-Oriented Documentation

The final documents must be useful to a client developer without reading backend code.

For every endpoint, make it easy to answer:

1. What URL do I call?
2. Which HTTP method?
3. Do I need authentication?
4. Which roles/permissions are required?
5. What query/path parameters exist?
6. What do I send?
7. Which fields are required?
8. Which values are accepted?
9. What do I receive?
10. Which errors should I handle?
11. What should the UI do after success?
12. Are there special behaviours or side effects?

If the generated endpoint documentation does not clearly answer these questions, improve it before finalizing.

---

# 35. Implementation-Oriented Documentation

The same endpoint must also allow another backend developer to understand:

1. Which guards execute?
2. Which validations occur?
3. Which service method is used?
4. Which database operations occur?
5. Which business rules are enforced?
6. Which external services are called?
7. Which notifications/events are triggered?
8. Which errors can occur?
9. Which response transformation occurs?
10. Which unusual implementation details exist?

Capture these under `Backend Behaviour` rather than mixing them into the client instructions.

---

# 36. Module Completeness Check

Before finalizing each module, verify:

- [ ] Every implemented route appears in the Endpoint Index.
- [ ] Every indexed route has full documentation.
- [ ] Endpoint numbering matches the index.
- [ ] HTTP methods are correct.
- [ ] Full API paths are correct.
- [ ] Authentication is correct.
- [ ] Roles are complete.
- [ ] Permissions are complete.
- [ ] Query parameters are complete.
- [ ] Path parameters are complete.
- [ ] Body fields are complete.
- [ ] Every type-dependent/discriminated body lists all supported discriminator values.
- [ ] Every type-dependent/discriminated body includes a required/optional/not-applicable field matrix.
- [ ] Every supported discriminator value has a complete valid request example.
- [ ] Type-specific files, validation, enums, response fields, and update rules are documented.
- [ ] Optional/required states are correct.
- [ ] Enum values are complete.
- [ ] Validation rules are implementation-faithful.
- [ ] File requirements are complete.
- [ ] Success status codes are correct.
- [ ] Success response examples are complete.
- [ ] Error variants are complete.
- [ ] Error JSON is complete.
- [ ] Client Integration exists for every endpoint.
- [ ] Backend Behaviour exists for every endpoint.
- [ ] Important Behaviour appears only when needed.
- [ ] Side effects are documented.
- [ ] Security-sensitive behaviour is documented.
- [ ] No route is invented.
- [ ] No field is invented.
- [ ] No response is invented.
- [ ] No permission is invented.
- [ ] No important implementation behaviour is omitted.

Do not mark the module complete until all applicable checks pass.

---

# 37. Visual Quality Check

Before finalizing every Markdown document, inspect the document as a reader would.

Verify:

- [ ] The module title is immediately understandable.
- [ ] The Overview is concise.
- [ ] The Endpoint Index makes navigation easy.
- [ ] Endpoints are visually separated.
- [ ] Each endpoint can be scanned quickly.
- [ ] Request fields are presented in tables.
- [ ] JSON blocks are easy to read.
- [ ] Client instructions are separate from backend internals.
- [ ] Long sections are broken into meaningful subsections.
- [ ] There are no unnecessary duplicate explanations.
- [ ] Heading hierarchy is consistent.
- [ ] There are no giant unbroken paragraphs.
- [ ] The document looks polished in GitHub Markdown preview.

Accuracy takes priority over aesthetics, but poor readability must still be corrected.

---

# 38. Cross-Module Consistency Check

After all modules are generated:

Verify that the same concepts use the same terminology throughout.

Examples:

- Always use `Bearer <access-token>` if that is the project convention.
- Do not alternate randomly between `Auth`, `Authentication`, and `Authorization`.
- Use the same capitalization for roles.
- Use the same API path formatting.
- Use the same success/error response layout.
- Use the same field-table structure.
- Use the same Client Integration layout.
- Use the same Backend Behaviour layout.

Do not normalize actual implementation differences.

Consistency applies to documentation presentation, not to changing backend behaviour.

---

# 39. Final Directory Verification

Before finishing, compare the generated directory against the actual backend modules and controllers.

The final output must contain:

```text
implementation-docs/
├── README.md
├── 1-<module>.md
├── 2-<module>.md
├── 3-<module>.md
└── ...
```

Verify:

- Every implemented module is represented.
- Every implemented route is represented.
- File numbering is continuous.
- File names are descriptive.
- README links match generated filenames.
- Endpoint counts in README match module files.
- Endpoint counts in module indexes match actual documented routes.

---

# 40. Final Requirement

The final output is not complete merely because Markdown files were created.

It is complete only when:

1. Every implemented module has been inspected.
2. Every route has been traced through the actual implementation.
3. Every route has been documented.
4. Every request shape has been verified.
5. Every response shape has been verified.
6. Every JSON example is valid.
7. Every relevant authorization rule is documented.
8. Every important error branch is documented.
9. Every client-facing behaviour is documented.
10. Every backend behaviour is implementation-faithful.
11. The documentation is visually consistent and easy to scan.
12. The completeness checklist passes.
13. The visual-quality checklist passes.
14. The cross-module consistency check passes.

**Do not sacrifice implementation accuracy for prettier documentation.**

**Do not sacrifice readability simply because the implementation is complex.**

The final result should look and read like polished professional API documentation generated directly from the actual backend implementation.
