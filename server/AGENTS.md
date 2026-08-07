# Server Instructions

## Scope and Stack

- These rules apply to `server/` and extend the root `AGENTS.md`.
- Keep the current backend stack: JavaScript with ES modules, Node.js, Express, MongoDB/Mongoose, JWT, Multer, CORS, Helmet, and the existing migration tooling.
- Keep routes, controllers, middleware, models, validators, helpers, and configuration organized according to existing server patterns.

## CURRENT PROJECT ARCHITECTURE

- Express mounts routes in `server.js`; the 404 middleware and global `errorHandler` are mounted last.
- Controllers already use `src/helpers/asyncHandler.js` to forward rejected async work to the centralized error middleware.
- The existing API uses JSON success/error response shapes and JWT Bearer-token authentication.
- Authentication currently validates JWT tokens in middleware. Do not replace it with a new session, refresh-token, or cookie architecture unless a separate task explicitly requests that change.
- Mongoose models, server-side validation, Multer uploads, CORS, Helmet, rate limiting, and environment-based configuration are already part of the application.

## PREFERRED PRACTICES FOR NEW CODE

- Keep controllers thin. Put reusable or complex business logic in services or helpers only when the extraction improves clarity.
- Use the centralized error middleware for unhandled controller/service errors.
- Wrap async controllers with the existing `asyncHandler` (or `catchAsync` if the project uses it). Do not duplicate controller-level `try/catch` blocks solely to send standard error responses.
- Use `try/catch` locally only when an operation needs local recovery, cleanup, compensation, or an error transformation before rethrowing.
- For expected HTTP errors, throw `createHttpError(statusCode, message)` when the project provides that helper. If it is not available, do not add a dependency or change the error architecture as part of an unrelated task; make that change only when explicitly requested. Until the project explicitly adopts an HTTP-error helper, follow the existing response/error pattern used by adjacent controllers and middleware; do not introduce a second error mechanism in an unrelated task.
- Use early returns for validation and authorization preconditions. The centralized handler owns unexpected server errors.
- Return safe client-facing messages. Do not expose stack traces, database details, secrets, or other internal information outside development.

## API, Validation, and Database

- Follow existing route naming and REST conventions. Prefer plural resource names for new isolated endpoints when this does not change the existing API contract. Keep related endpoints in their existing route modules and apply middleware at the route level when needed.
- Validate and sanitize input at the boundary, use Mongoose schema validation, and prevent NoSQL injection.
- Use descriptive schema fields, appropriate types, required/default values, and indexes for fields that are queried frequently.
- Use `.lean()` for read-only Mongoose queries when a document instance is not required. Use `.select()`, limits, and `.populate()` deliberately to avoid unnecessary data and N+1-style work.
- Use transactions when atomicity across multiple documents is required and the configured MongoDB deployment supports them; otherwise follow the existing persistence pattern.

## Security and Operations

- Keep secrets in environment variables and never commit `.env` files. Do not reveal sensitive configuration in responses or logs.
- Preserve the existing JWT verification, CORS, Helmet, rate limiting, upload validation, and static-upload behavior unless the task explicitly changes them.
- Validate upload type and size and handle Multer errors safely.
- Keep database connection errors observable on the server and keep API error responses safe for clients.

## Backend Quality Checks

- Test successful, validation, authentication/authorization, database-failure, and edge-case API paths when they are affected.
- Check HTTP status codes, response shapes, middleware order, route compatibility, and downstream consumers.
- Do not add a dependency, rewrite authentication, or change the API contract without explicit authorization.
