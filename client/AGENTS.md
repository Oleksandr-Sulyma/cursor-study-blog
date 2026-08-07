# Client Instructions

## Scope and Stack

- These rules apply to `client/` and extend the root `AGENTS.md`.
- Keep the current frontend stack: JavaScript, React, Vite, Ant Design, Axios, React Router, React Hot Toast, and the existing custom hooks/API layer.
- Use functional React components and hooks; do not introduce class components, TypeScript, CSS-in-JS, styled-components, or another UI library.

## CURRENT PROJECT ARCHITECTURE

- The root app configures Ant Design through `ConfigProvider` in `src/App.jsx`.
- API modules use the shared `src/api/axiosConfig.js` axios instance. It centralizes `baseURL`, `withCredentials`, the Authorization header, request Content-Type behavior, and response handling for 401 errors.
- The current authentication implementation stores an access token in `localStorage` and sends it as a Bearer token through the axios interceptor.
- React Context and custom hooks provide shared state and API-request behavior where they are already used.

## PREFERRED PRACTICES FOR NEW CODE

- Preserve the current token architecture unless a separate task explicitly changes authentication. Do not treat `localStorage` as the preferred design for a new authentication system; use a safer httpOnly-cookie/session design only when that architectural work is explicitly requested.
- Use the existing axios instance and existing API modules. Do not create per-component axios configuration.
- Keep `baseURL`, headers, credentials, and interceptors centralized in the axios instance.
- Do not mix Axios and `fetch` without a concrete reason and a consistent handling strategy.
- Use the existing API and hook patterns before adding a new one.

## Ant Design and Styling

- Use Ant Design components for UI when Ant Design provides the needed capability. Do not mix UI libraries.
- Use Ant Design layout, Grid, Space, Form, feedback components, and responsive props where appropriate.
- Component-level colors and spacing must use Ant Design theme tokens, component props, and the existing design system; do not introduce arbitrary color values or spacing scales.
- Centralized theme configuration in `ConfigProvider` is allowed. Custom tokens such as `colorPrimary` and component tokens belong there, not scattered through components.
- Do not add Tailwind utility classes or Tailwind-based examples. Do not replace the existing Ant Design theme with a different styling system.

## React Components and Hooks

- Keep components focused and prefer composition over inheritance.
- Call hooks only at the top level of React components or custom hooks. Custom hooks must start with `use`.
- Keep state local until it must be shared; use the existing Context or hook patterns for existing global concerns.
- Use `useCallback`, `useMemo`, and `React.memo` only when referential stability or an expensive computation/render makes them useful. They are not mandatory for every callback, value, or component.
- Avoid expensive work during render, unnecessary effects, nested ternaries, direct state mutation, and missing `useEffect` cleanup when a subscription or resource requires it.
- Use lazy loading and code splitting for non-critical routes or components only when it provides a real benefit.

## Forms, Requests, and Errors

- Use controlled forms where the established component pattern does so. Handle submission loading states and validate on the client while keeping server validation authoritative.
- Present user-friendly errors through the existing feedback mechanism and keep technical details out of user-facing messages.
- Handle request failures at the level that can recover or inform the user. Preserve interceptor behavior rather than duplicating global authentication handling.
- Keep API calls out of presentational components when an existing API module or custom hook is the appropriate layer.

## Frontend Quality Checks

- Keep React files and names consistent with adjacent code, use PascalCase component names, and avoid unused imports.
- Verify responsive behavior for UI changes and use Ant Design's responsive Grid or component props.
- Before handoff, run the available client lint/build checks when relevant and inspect affected loading, empty, and error states.
