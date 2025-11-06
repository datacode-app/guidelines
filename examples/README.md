# Examples Directory

This directory contains real-world examples demonstrating how to apply our engineering guidelines in practice.

## Purpose

Reading guidelines is one thing. Seeing them in action is another. These examples show:
- ✅ What good code looks like
- ❌ Common mistakes to avoid
- 🔄 Before/after refactoring examples
- 💡 Best practices in context

## Structure

```
examples/
├── code-style/           # Code style examples
│   ├── good/            # Well-styled code
│   └── bad/             # Anti-patterns
├── testing/              # Testing examples
│   ├── unit/            # Unit test examples
│   ├── integration/     # Integration test examples
│   └── e2e/             # E2E test examples
├── api-design/           # API design examples
│   ├── rest/            # RESTful API examples
│   └── graphql/         # GraphQL examples (if applicable)
├── security/             # Security examples
│   ├── vulnerable/      # Vulnerable code (educational)
│   └── secure/          # Secured version
├── git-workflow/         # Git workflow examples
│   ├── commits/         # Example commit messages
│   └── prs/             # Example PRs
└── refactoring/          # Refactoring examples
    ├── before/          # Code before refactoring
    └── after/           # Code after refactoring
```

## Examples Index

### Code Style

#### Good Examples
- [TypeScript Component](code-style/good/UserProfile.tsx) - Well-structured React component
- [API Controller](code-style/good/UserController.ts) - Clean API controller
- [Utility Function](code-style/good/dateFormatter.ts) - Pure utility function
- [Custom Hook](code-style/good/useAuth.ts) - React hook following best practices

#### Anti-Patterns
- [God Object](code-style/bad/EverythingManager.ts) - Overly complex class
- [Unclear Naming](code-style/bad/ConfusingNames.ts) - Poor variable names
- [Magic Numbers](code-style/bad/MagicNumbers.ts) - Hardcoded values
- [Deep Nesting](code-style/bad/DeepNesting.ts) - Excessive indentation

### Testing

#### Unit Tests
- [Simple Function Test](testing/unit/math.test.ts) - Testing pure functions
- [Component Test](testing/unit/Button.test.tsx) - Testing React components
- [Service Test](testing/unit/UserService.test.ts) - Testing business logic
- [Mock Example](testing/unit/ApiClient.test.ts) - Using mocks effectively

#### Integration Tests
- [API Integration](testing/integration/userApi.test.ts) - Testing API endpoints
- [Database Integration](testing/integration/userRepository.test.ts) - Testing DB queries

#### E2E Tests
- [User Flow](testing/e2e/login.spec.ts) - Complete user journey test

### API Design

#### RESTful APIs
- [User API](api-design/rest/users.ts) - CRUD operations
- [Pagination](api-design/rest/pagination.ts) - Implementing pagination
- [Error Handling](api-design/rest/errorHandling.ts) - Consistent error responses
- [Versioning](api-design/rest/versioning.ts) - API versioning strategy

### Security

#### Vulnerable Code (Educational)
- [SQL Injection](security/vulnerable/sqlInjection.ts) - Dangerous query building
- [XSS Vulnerability](security/vulnerable/xss.tsx) - Unescaped user input
- [Exposed Secrets](security/vulnerable/secrets.ts) - Hardcoded credentials
- [Missing Auth](security/vulnerable/noAuth.ts) - Unprotected endpoints

#### Secure Code
- [SQL Injection Prevention](security/secure/parameterizedQueries.ts) - Safe queries
- [XSS Prevention](security/secure/escapedOutput.tsx) - Sanitized output
- [Secrets Management](security/secure/envVariables.ts) - Proper secret handling
- [Authentication](security/secure/authentication.ts) - Proper auth implementation

### Git Workflow

#### Commit Messages
```
feat(auth): add OAuth2 login support

Implemented OAuth2 authentication flow using Google and GitHub providers.
Users can now sign in with their existing accounts instead of creating
new credentials.

- Added OAuth provider configuration
- Implemented callback handlers
- Added user profile sync
- Updated login UI with provider buttons

Closes #234

---

fix(payments): handle declined card responses

Previously, declined cards would cause 500 errors. Now properly
handled with clear user-facing error messages.

Fixes #456

---

refactor(dashboard): simplify chart rendering logic

Extracted chart configuration into separate module and reduced
component complexity from 250 lines to 80 lines. No functional changes.

---

docs(api): update authentication endpoints documentation

Updated OpenAPI spec to reflect new OAuth2 endpoints and deprecated
legacy basic auth.

---

test(users): add integration tests for user CRUD operations

Improved coverage from 65% to 85% for user service.
```

#### Pull Request Examples
See `git-workflow/prs/` for example PR descriptions that follow our template.

### Refactoring

#### Before/After Examples
- [Complex Function](refactoring/ComplexCalculation.md) - Simplifying complex logic
- [God Class](refactoring/Refactoring-God-Class.md) - Breaking down large classes
- [Callback Hell](refactoring/Async-Refactoring.md) - Converting to async/await

## Using These Examples

### For Learning
1. Start with "good" examples to see best practices
2. Study "bad" examples to recognize anti-patterns
3. Compare before/after in refactoring examples

### For Code Reviews
Link to specific examples when providing feedback:
```
💡 SUGGESTION: This could be simplified. See example:
https://github.com/datacode-app/guidelines/blob/main/examples/refactoring/ComplexCalculation.md
```

### For Onboarding
New engineers should review:
1. Code style good examples
2. Testing examples (unit → integration → e2e)
3. Security secure examples
4. Git workflow commit messages

## Contributing Examples

Found a great real-world example? Add it!

1. Create file in appropriate directory
2. Add comments explaining the why, not just the what
3. Link to relevant guideline sections
4. Update this README
5. Submit PR

### Example Template

```typescript
/**
 * Example: [Brief Description]
 *
 * Guideline: [Link to relevant guideline]
 *
 * This example demonstrates:
 * - [Key concept 1]
 * - [Key concept 2]
 *
 * Why this approach:
 * - [Reason 1]
 * - [Reason 2]
 */

// Code example here
```

## Notes

- **Vulnerable code examples**: Only for education. Never copy to production!
- **Simplified for clarity**: Real production code may have additional complexity
- **Language-specific**: Most examples are TypeScript, adapt for your language
- **Context matters**: Examples show one way, not the only way

## Quick Reference

| Need example of... | Look in... |
|-------------------|------------|
| Component structure | `code-style/good/UserProfile.tsx` |
| Writing tests | `testing/unit/*.test.ts` |
| API endpoint | `api-design/rest/users.ts` |
| Security vulnerability | `security/vulnerable/*.ts` |
| How to fix security issue | `security/secure/*.ts` |
| Good commit message | `git-workflow/commits/` |
| Refactoring approach | `refactoring/*.md` |

## Support

Questions about examples? Ask in #engineering Slack.

Want more examples? Request in GitHub issues.
