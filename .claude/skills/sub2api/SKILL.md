```markdown
# sub2api Development Patterns

> Auto-generated skill from repository analysis

## Overview

This skill describes the key development patterns, coding conventions, and workflows used in the `sub2api` repository. The codebase is primarily written in Go (Golang) for backend services, with a TypeScript/Vue.js frontend. The repository emphasizes clear commit messages, robust testing, and a well-structured approach to feature development, bugfixing, schema migrations, and documentation updates. This guide will help you contribute effectively and consistently to the project.

## Coding Conventions

**File Naming**
- Use `snake_case` for Go source files.
  - Example: `user_service.go`, `order_repository.go`
- Test files use the `_test.go` suffix.
  - Example: `user_service_test.go`

**Imports**
- Use relative import paths in Go.
  - Example:
    ```go
    import "../repository"
    ```

**Exports**
- Use named exports for Go functions, types, and variables.
  - Example:
    ```go
    func NewUserService() *UserService {
        // ...
    }
    ```

**Commit Messages**
- Prefix with `fix`, `feat`, or `chore` as appropriate.
- Keep messages concise (average ~57 characters).
  - Example: `fix: handle nil pointer in user repository`

## Workflows

### Backend Bugfix with Unit Tests
**Trigger:** When fixing a backend bug and ensuring it is covered by tests  
**Command:** `/fix-with-test`

1. Identify and fix the bug in backend service, repository, or handler code.
2. Update or add unit/integration tests that cover the bug scenario.
3. Commit both code and test changes together.

**Files Involved:**
- `backend/internal/service/*.go`
- `backend/internal/service/*_test.go`
- `backend/internal/repository/*.go`
- `backend/internal/repository/*_test.go`
- `backend/internal/handler/*.go`
- `backend/internal/handler/*_test.go`

**Example:**
```go
// backend/internal/service/user_service.go
func (s *UserService) GetUser(id int) (*User, error) {
    if id <= 0 {
        return nil, errors.New("invalid user id")
    }
    // ...
}
```
```go
// backend/internal/service/user_service_test.go
func TestGetUser_InvalidID(t *testing.T) {
    _, err := svc.GetUser(0)
    assert.Error(t, err)
}
```

---

### Backend Feature Plus Frontend and i18n
**Trigger:** When adding a new feature that requires both backend and frontend changes  
**Command:** `/feature-fullstack`

1. Implement backend logic (service, handler, repository).
2. Expose new or updated API endpoints.
3. Update or add frontend components/views to consume the new API.
4. Update frontend API client files.
5. Update i18n translation files for new UI strings.
6. Add or update frontend tests if needed.

**Files Involved:**
- `backend/internal/service/*.go`
- `backend/internal/handler/*.go`
- `backend/internal/repository/*.go`
- `frontend/src/api/**/*.ts`
- `frontend/src/views/**/*.vue`
- `frontend/src/components/**/*.vue`
- `frontend/src/i18n/locales/*.ts`

**Example:**
```go
// backend/internal/handler/user_handler.go
func (h *UserHandler) CreateUser(c *gin.Context) {
    // ...
}
```
```typescript
// frontend/src/api/user.ts
export function createUser(data) {
    return http.post('/api/user', data)
}
```
```vue
<!-- frontend/src/views/UserCreate.vue -->
<template>
  <form @submit="submit">
    <!-- ... -->
  </form>
</template>
```
```ts
// frontend/src/i18n/locales/en.ts
export default {
  user: {
    createSuccess: "User created successfully"
  }
}
```

---

### Schema Migration with ent and SQL
**Trigger:** When changing the database schema (add/modify tables/fields/relations)  
**Command:** `/schema-change`

1. Edit ent schema files (`backend/ent/schema/*.go`).
2. Regenerate ent code (`backend/ent/*.go`, etc.).
3. Create new SQL migration file (`backend/migrations/*.sql`).
4. Update backend repository/service/handler code to use the new schema.
5. Update or add integration/unit tests.
6. Update frontend API/types if relevant.

**Files Involved:**
- `backend/ent/schema/*.go`
- `backend/ent/*.go`
- `backend/ent/*/*.go`
- `backend/ent/migrate/schema.go`
- `backend/migrations/*.sql`
- `backend/internal/repository/*.go`
- `backend/internal/service/*.go`
- `frontend/src/api/**/*.ts`
- `frontend/src/types/**/*.ts`

**Example:**
```go
// backend/ent/schema/user.go
type User struct {
    ent.Schema
}

func (User) Fields() []ent.Field {
    return []ent.Field{
        field.String("email").Unique(),
        field.String("name"),
    }
}
```
```sql
-- backend/migrations/20240401_add_user_name.sql
ALTER TABLE users ADD COLUMN name VARCHAR(255);
```

---

### Fix Content-Type or Error Handling in Gateway
**Trigger:** When fixing HTTP Content-Type or error response handling in gateway/service/handler  
**Command:** `/fix-gateway-error`

1. Identify the protocol or error-handling bug.
2. Update gateway/service/handler code to fix Content-Type or error propagation.
3. Add or update regression/unit tests to cover the edge case.
4. Commit code and tests together.

**Files Involved:**
- `backend/internal/service/gateway_*.go`
- `backend/internal/handler/gateway_*.go`
- `backend/internal/service/openai_gateway_*.go`
- `backend/internal/handler/openai_gateway_*.go`
- `backend/internal/service/*_test.go`
- `backend/internal/handler/*_test.go`

**Example:**
```go
// backend/internal/handler/gateway_handler.go
c.Header("Content-Type", "application/json")
c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
```

---

### README or Sponsor Assets Update
**Trigger:** When updating documentation or sponsor/partner logo assets  
**Command:** `/update-readme`

1. Edit `README.md` and/or localized README files.
2. Add, update, or remove logo images in `assets/partners/logos`.
3. Commit documentation and asset changes together.

**Files Involved:**
- `README.md`
- `README_CN.md`
- `README_JA.md`
- `assets/partners/logos/*.png`
- `assets/partners/logos/*.jpg`

---

### Merge PR for Existing Bugfix or Feature
**Trigger:** When merging a PR that implements a bugfix or feature, matching a previous direct commit  
**Command:** `/merge-pr`

1. Review and approve the PR.
2. Merge the PR into the main branch.
3. Ensure PR commit matches files and message of a previous direct commit.

**Files Involved:**
- `backend/internal/service/*.go`
- `backend/internal/service/*_test.go`
- `backend/internal/handler/*.go`
- `backend/internal/handler/*_test.go`
- `backend/internal/repository/*.go`
- `frontend/src/api/**/*.ts`
- `frontend/src/views/**/*.vue`
- `frontend/src/i18n/locales/*.ts`

---

## Testing Patterns

- **Framework:** [vitest](https://vitest.dev/) (for frontend TypeScript)
- **File Pattern:** `*.spec.ts`
- **Backend Go Tests:** Use Go's built-in testing (`*_test.go` files).
- **Test Placement:** Place tests alongside the code they test.

**Example (Go):**
```go
// backend/internal/service/user_service_test.go
func TestUserService_CreateUser(t *testing.T) {
    // ...
}
```

**Example (TypeScript):**
```typescript
// frontend/src/api/user.spec.ts
import { createUser } from './user'

test('createUser sends correct payload', async () => {
  // ...
})
```

## Commands

| Command              | Purpose                                                      |
|----------------------|--------------------------------------------------------------|
| /fix-with-test       | Fix a backend bug and add/update corresponding tests         |
| /feature-fullstack   | Implement a new backend feature with frontend and i18n       |
| /schema-change       | Add/modify database schema and related code/tests            |
| /fix-gateway-error   | Fix Content-Type or error handling in gateway/service/handler|
| /update-readme       | Update documentation or sponsor logo assets                  |
| /merge-pr            | Merge a PR for an existing bugfix or feature                 |
```
