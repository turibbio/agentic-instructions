---
description: '[FRAMEWORK] development standards and best practices'
applyTo: '**/*.[FRONTEND_EXT], **/*.[FRONTEND_SCRIPT_EXT], **/*.[STYLE_EXT], **/*.[STYLE_ALT_EXT]'
---

# [FRAMEWORK] Development Instructions

Instructions for building high-quality [FRAMEWORK] applications with modern patterns, hooks, and best practices following the official [FRAMEWORK] documentation.

## Project Context
- Latest [FRAMEWORK] version ([FRAMEWORK] [VERSION]+)
- [TYPE_SYSTEM] for type safety (when applicable)
- Functional components with hooks as default
- Follow [FRAMEWORK]'s official style guide and best practices
- Use modern build tools ([BUILD_TOOL], Create [FRAMEWORK] App, or custom Webpack setup)
- Implement proper component composition and reusability patterns

## Development Standards

### Architecture
- Use functional components with hooks as the primary pattern
- Implement component composition over inheritance
- Organize components by feature or domain for scalability
- Separate presentational and container components clearly
- Use custom hooks for reusable stateful logic
- Implement proper component hierarchies with clear data flow

### [TYPE_SYSTEM] Integration
- Use [TYPE_SYSTEM] interfaces for props, state, and component definitions
- Define proper types for event handlers and refs
- Implement generic components where appropriate
- Use strict mode in `[TYPE_CONFIG_FILE]` for type safety
- Leverage [FRAMEWORK]'s built-in types (`[FRAMEWORK].[COMPONENT_TYPE]`, `[FRAMEWORK].ComponentProps`, etc.)
- Create union types for component variants and states

### Component Design
- Follow the single responsibility principle for components
- Use descriptive and consistent naming conventions
- Implement proper prop validation with [TYPE_SYSTEM] or PropTypes
- Design components to be testable and reusable
- Keep components small and focused on a single concern
- Use composition patterns (render props, children as functions)

### State Management
- Use `useState` for local component state
- Implement `useReducer` for complex state logic
- Leverage `useContext` for sharing state across component trees
- Consider external state management ([STATE_LIB_1], [STATE_LIB_2]) for complex applications
- Implement proper state normalization and data structures
- Use [DATA_FETCHING_LIB] or [DATA_FETCHING_ALT] for server state management

### Hooks and Effects
- Use `useEffect` with proper dependency arrays to avoid infinite loops
- Implement cleanup functions in effects to prevent memory leaks
- Use `useMemo` and `useCallback` for performance optimization when needed
- Create custom hooks for reusable stateful logic
- Follow the rules of hooks (only call at the top level)
- Use `useRef` for accessing DOM elements and storing mutable values

### Styling
- Use CSS Modules, Styled Components, or modern CSS-in-JS solutions
- Implement responsive design with mobile-first approach
- Follow BEM methodology or similar naming conventions for CSS classes
- Use CSS custom properties (variables) for theming
- Implement consistent spacing, typography, and color systems
- Ensure accessibility with proper ARIA attributes and semantic HTML

### Performance Optimization
- Use `[FRAMEWORK].memo` for component memoization when appropriate
- Implement code splitting with `[FRAMEWORK].lazy` and `Suspense`
- Optimize bundle size with tree shaking and dynamic imports
- Use `useMemo` and `useCallback` judiciously to prevent unnecessary re-renders
- Implement virtual scrolling for large lists
- Profile components with [FRAMEWORK] DevTools to identify performance bottlenecks

### Data Fetching
- Use modern data fetching libraries ([DATA_FETCHING_LIB], [DATA_FETCHING_ALT], [GRAPHQL_CLIENT])
- Implement proper loading, error, and success states
- Handle race conditions and request cancellation
- Use optimistic updates for better user experience
- Implement proper caching strategies
- Handle offline scenarios and network errors gracefully

### Error Handling
- Implement Error Boundaries for component-level error handling
- Use proper error states in data fetching
- Implement fallback UI for error scenarios
- Log errors appropriately for debugging
- Handle async errors in effects and event handlers
- Provide meaningful error messages to users

### Forms and Validation
- Use controlled components for form inputs
- Implement proper form validation with libraries like [FORM_LIB_1], [FORM_LIB_2]
- Handle form submission and error states appropriately
- Implement accessibility features for forms (labels, ARIA attributes)
- Use debounced validation for better user experience
- Handle file uploads and complex form scenarios

### Routing
- Use [ROUTER_LIB] for client-side routing
- Implement nested routes and route protection
- Handle route parameters and query strings properly
- Implement lazy loading for route-based code splitting
- Use proper navigation patterns and back button handling
- Implement breadcrumbs and navigation state management

### Testing
- Write unit tests for components using [TESTING_LIB]
- Test component behavior, not implementation details
- Use [TEST_RUNNER] for test runner and assertion library
- Implement integration tests for complex component interactions
- Mock external dependencies and API calls appropriately
- Test accessibility features and keyboard navigation

### Security
- Sanitize user inputs to prevent XSS attacks
- Validate and escape data before rendering
- Use HTTPS for all external API calls
- Implement proper authentication and authorization patterns
- Avoid storing sensitive data in localStorage or sessionStorage
- Use Content Security Policy (CSP) headers

### Accessibility
- Use semantic HTML elements appropriately
- Implement proper ARIA attributes and roles
- Ensure keyboard navigation works for all interactive elements
- Provide alt text for images and descriptive text for icons
- Implement proper color contrast ratios
- Test with screen readers and accessibility tools

## Implementation Process
1. Plan component architecture and data flow
2. Set up project structure with proper folder organization
3. Define [TYPE_SYSTEM] interfaces and types
4. Implement core components with proper styling
5. Add state management and data fetching logic
6. Implement routing and navigation
7. Add form handling and validation
8. Implement error handling and loading states
9. Add testing coverage for components and functionality
10. Optimize performance and bundle size
11. Ensure accessibility compliance
12. Add documentation and code comments

## Additional Guidelines
- Follow [FRAMEWORK]'s naming conventions (PascalCase for components, camelCase for functions)
- Use meaningful commit messages and maintain clean git history
- Implement proper code splitting and lazy loading strategies
- Document complex components and custom hooks with JSDoc
- Use [LINTER] and [FORMATTER] for consistent code formatting
- Keep dependencies up to date and audit for security vulnerabilities
- Implement proper environment configuration for different deployment stages
- Use [FRAMEWORK] Developer Tools for debugging and performance analysis

## Common Patterns
- Higher-Order Components (HOCs) for cross-cutting concerns
- Render props pattern for component composition
- Compound components for related functionality
- Provider pattern for context-based state sharing
- Container/Presentational component separation
- Custom hooks for reusable logic extraction