# Codex Guidelines for StudySprint

## Scope of These Instructions

- This file contains project-wide rules. Read the nearest nested `AGENTS.md` before editing files in `client/` or `server/`.
- Inspect the existing implementation before proposing or making a change. Follow established patterns, naming, and structure.
- Do not change the application stack, public API, data model, authentication architecture, or dependencies unless the task explicitly requires it.
- Prefer complete, focused solutions with appropriate validation and error handling. Do not over-engineer simple work.
- Do not commit secrets, credentials, or environment files.

## CURRENT PROJECT ARCHITECTURE

- The application is a MERN project: JavaScript, React with Vite, Ant Design, Axios, Node.js, Express, and MongoDB/Mongoose.
- The client and server are separate applications under `client/` and `server/`.
- The client communicates with the server through an API layer. The server owns routing, business logic, authentication, persistence, uploads, and external integrations.
- File uploads use Multer and local server storage. Deployment configuration targets separate client and server deployments.
- The current implementation must be preserved unless a task explicitly authorizes an architectural change.

## PREFERRED PRACTICES FOR NEW CODE

- First look for an existing component, hook, API function, route, controller, helper, or model that solves a similar problem.
- Keep changes compatible and incremental. Update dependent code only when the requested change requires it.
- Extract repeated or complex business logic only when doing so improves clarity without creating unnecessary abstractions.
- Keep comments brief and limited to non-obvious logic. Prefer clear naming and small focused functions.
- For a feature spanning client and server, verify both sides and test the API before completing the UI integration.

## JavaScript Style and Structure

- Use modern JavaScript with ES modules. Do not add TypeScript to this project.
- Use 2-space indentation, single quotes, no semicolons unless needed for syntax, camelCase for variables and functions, and PascalCase for React components.
- Use strict equality, early returns, guard clauses, descriptive names, and `is`/`has`/`should`/`can` prefixes for booleans.
- Keep files ordered as imports, exported component or function, subcomponents, helpers, then constants/static content when that order fits the file.
- Avoid unused imports and variables. Use named exports by default; reserve default exports for pages, routes, or an established existing pattern.

## Async Operations

- Use `async/await` and handle errors through the mechanism appropriate to the layer.
- Run independent asynchronous operations concurrently with `Promise.all`.
- Run dependent operations sequentially when a later operation needs an earlier result.
- Use `Promise.allSettled` when every operation's result is required even if one or more operations fail.
- Do not use `Promise.all` for dependent operations or when partial results must be processed after failures.

## Quality, Security, and Review

- Validate external input, preserve REST conventions, use appropriate HTTP status codes, and avoid exposing secrets or internal implementation details.
- Before handoff, check imports, likely runtime paths, affected client/server behavior, and relevant lint or test commands.
- During review, check for missing validation or error handling, security issues, duplicate logic, N+1 queries, race conditions, unnecessary dependencies, and performance regressions.
- For bug fixes, identify the root cause, inspect relevant client and server behavior, and check for the same issue elsewhere.

## Repository Safety

- Do not modify files unrelated to the current task.
- Preserve existing uncommitted user changes and never overwrite or revert them unless explicitly requested.
- Do not run destructive Git operations or perform commit, push, merge, rebase, reset, branch deletion, or PR creation unless explicitly requested.

## Documentation

- Reference real project files and current configuration when relevant.
- Do not add or reference deprecated Cursor rule files; project instructions now live in these `AGENTS.md` files.
