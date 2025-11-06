# Testing Standards Guide

## Purpose
Comprehensive testing ensures code quality, prevents regressions, and enables confident refactoring. This guide defines our testing philosophy, standards, and best practices.

## Testing Philosophy

1. **Tests are documentation** - They show how code should be used
2. **Tests enable refactoring** - Change implementation without fear
3. **Tests prevent regressions** - Catch bugs before production
4. **Tests are code** - Maintain them with same care as production code
5. **Fast tests, fast feedback** - Optimize for quick feedback cycles

---

## Test Coverage Requirements

### Minimum Coverage Targets

| Code Type | Coverage Target | Enforcement |
|-----------|----------------|-------------|
| **Critical paths** (auth, payments) | 95%+ | Strict |
| **Business logic** | 85%+ | Required |
| **API endpoints** | 80%+ | Required |
| **UI components** | 75%+ | Recommended |
| **Utilities** | 90%+ | Required |

### Coverage is NOT Everything

✅ **Good:**
- Tests that verify actual behavior
- Tests that catch real bugs
- Tests that document use cases

❌ **Bad:**
- Tests written just to hit coverage %
- Tests that don't assert anything meaningful
- Flaky tests that randomly fail

---

## Types of Tests

### Test Pyramid

```
        /\
       /  \        E2E Tests (5%)
      /----\       - Slow, expensive
     /      \      - Full system verification
    /--------\     - Critical user journeys
   /          \
  /------------\   Integration Tests (20%)
 /              \  - API endpoints, DB queries
/----------------\ - Component integration

                   Unit Tests (75%)
                   - Fast, isolated
                   - Business logic
                   - Pure functions
```

### 1. Unit Tests

**Purpose**: Test individual functions/components in isolation

**Characteristics**:
- Fast (milliseconds)
- No external dependencies (DB, API, file system)
- Use mocks/stubs for dependencies
- Run on every commit

**Example**:
```javascript
// utils/validation.js
function isValidEmail(email) {
  const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return regex.test(email);
}

// utils/validation.test.js
describe('isValidEmail', () => {
  it('should return true for valid email', () => {
    expect(isValidEmail('user@example.com')).toBe(true);
  });

  it('should return false for invalid email', () => {
    expect(isValidEmail('invalid-email')).toBe(false);
  });

  it('should return false for empty string', () => {
    expect(isValidEmail('')).toBe(false);
  });

  it('should handle email with plus addressing', () => {
    expect(isValidEmail('user+tag@example.com')).toBe(true);
  });
});
```

### 2. Integration Tests

**Purpose**: Test how multiple components work together

**Characteristics**:
- Moderate speed (seconds)
- May use test database
- Test real integrations (DB, cache, etc.)
- Run before deployment

**Example**:
```javascript
// tests/integration/user.test.js
describe('User API', () => {
  let testDb;

  beforeAll(async () => {
    testDb = await setupTestDatabase();
  });

  afterAll(async () => {
    await teardownTestDatabase(testDb);
  });

  beforeEach(async () => {
    await testDb.clear();
  });

  it('should create user and retrieve it', async () => {
    // Create user
    const createRes = await request(app)
      .post('/api/users')
      .send({ email: 'test@example.com', name: 'Test User' });

    expect(createRes.status).toBe(201);
    const userId = createRes.body.id;

    // Retrieve user
    const getRes = await request(app).get(`/api/users/${userId}`);

    expect(getRes.status).toBe(200);
    expect(getRes.body).toMatchObject({
      id: userId,
      email: 'test@example.com',
      name: 'Test User'
    });
  });
});
```

### 3. End-to-End (E2E) Tests

**Purpose**: Test complete user workflows through the UI

**Characteristics**:
- Slow (minutes)
- Test in browser/real environment
- Cover critical user journeys
- Run before major releases

**Example**:
```javascript
// e2e/checkout.test.js
describe('Checkout Flow', () => {
  it('should complete purchase successfully', async () => {
    // Navigate to product page
    await page.goto('https://app.example.com/products/123');

    // Add to cart
    await page.click('[data-testid="add-to-cart"]');
    await expect(page).toHaveText('[data-testid="cart-count"]', '1');

    // Go to checkout
    await page.click('[data-testid="cart-icon"]');
    await page.click('[data-testid="checkout-button"]');

    // Fill payment info
    await page.fill('[data-testid="card-number"]', '4242424242424242');
    await page.fill('[data-testid="exp-date"]', '12/25');
    await page.fill('[data-testid="cvc"]', '123');

    // Complete purchase
    await page.click('[data-testid="pay-button"]');

    // Verify success
    await expect(page).toHaveURL(/.*\/order-confirmation/);
    await expect(page).toHaveText('[data-testid="success-message"]', 'Order placed successfully');
  });
});
```

---

## Writing Good Tests

### Test Structure: AAA Pattern

**Arrange - Act - Assert**

```javascript
describe('Calculator', () => {
  it('should add two numbers correctly', () => {
    // Arrange: Set up test data
    const calculator = new Calculator();
    const a = 5;
    const b = 3;

    // Act: Execute the behavior
    const result = calculator.add(a, b);

    // Assert: Verify the outcome
    expect(result).toBe(8);
  });
});
```

### Test Naming

✅ **Good test names:**
```javascript
it('should return user when valid ID is provided')
it('should throw error when user is not found')
it('should send email notification after successful purchase')
it('should prevent user from accessing other users\' data')
```

❌ **Bad test names:**
```javascript
it('test 1')
it('works')
it('should work correctly')
it('user test')
```

### What to Test

✅ **DO test:**
- Public API / interface
- Edge cases and boundaries
- Error conditions
- Business logic
- Critical paths

❌ **DON'T test:**
- Implementation details
- Third-party libraries (assume they work)
- Trivial getters/setters
- Private methods (test through public interface)

### Test Independence

Each test should be **independent and isolated**:

✅ **Good:**
```javascript
describe('TodoList', () => {
  let todoList;

  beforeEach(() => {
    // Fresh instance for each test
    todoList = new TodoList();
  });

  it('should add item', () => {
    todoList.add('Buy milk');
    expect(todoList.items).toHaveLength(1);
  });

  it('should remove item', () => {
    todoList.add('Buy milk');
    todoList.remove('Buy milk');
    expect(todoList.items).toHaveLength(0);
  });
});
```

❌ **Bad:**
```javascript
describe('TodoList', () => {
  const todoList = new TodoList(); // Shared state!

  it('should add item', () => {
    todoList.add('Buy milk');
    expect(todoList.items).toHaveLength(1);
  });

  it('should remove item', () => {
    // This test depends on previous test running first!
    todoList.remove('Buy milk');
    expect(todoList.items).toHaveLength(0);
  });
});
```

### Avoid Test Logic

❌ **Bad - has logic:**
```javascript
it('should calculate total', () => {
  let total = 0;
  for (const item of cart.items) {
    total += item.price;
  }
  expect(cart.total()).toBe(total);
});
```

✅ **Good - explicit values:**
```javascript
it('should calculate total', () => {
  cart.add({ name: 'Item 1', price: 10 });
  cart.add({ name: 'Item 2', price: 20 });
  expect(cart.total()).toBe(30);
});
```

---

## Mocking & Stubbing

### When to Mock

✅ **Mock:**
- External APIs
- Database calls (in unit tests)
- File system operations
- Time-dependent code
- Random number generation
- Email/SMS services

❌ **Don't mock:**
- Code you're testing
- Simple utilities
- Everything (over-mocking makes tests brittle)

### Mocking Examples

#### Mock External API
```javascript
// services/weather.js
async function getWeather(city) {
  const response = await fetch(`https://api.weather.com/${city}`);
  return response.json();
}

// services/weather.test.js
jest.mock('node-fetch');

describe('getWeather', () => {
  it('should return weather data', async () => {
    // Mock fetch response
    fetch.mockResolvedValue({
      json: async () => ({ temp: 72, condition: 'sunny' })
    });

    const weather = await getWeather('Seattle');

    expect(weather).toEqual({ temp: 72, condition: 'sunny' });
    expect(fetch).toHaveBeenCalledWith('https://api.weather.com/Seattle');
  });
});
```

#### Mock Database
```javascript
// Use dependency injection for easier mocking
class UserService {
  constructor(database) {
    this.db = database;
  }

  async getUser(id) {
    return this.db.query('SELECT * FROM users WHERE id = $1', [id]);
  }
}

// Test
describe('UserService', () => {
  it('should retrieve user by ID', async () => {
    const mockDb = {
      query: jest.fn().mockResolvedValue({ id: 1, name: 'John' })
    };

    const service = new UserService(mockDb);
    const user = await service.getUser(1);

    expect(user).toEqual({ id: 1, name: 'John' });
    expect(mockDb.query).toHaveBeenCalledWith(
      'SELECT * FROM users WHERE id = $1',
      [1]
    );
  });
});
```

#### Mock Time
```javascript
// Code that depends on current time
function isExpired(expiryDate) {
  return new Date() > expiryDate;
}

// Test
describe('isExpired', () => {
  beforeEach(() => {
    jest.useFakeTimers();
    jest.setSystemTime(new Date('2024-03-16'));
  });

  afterEach(() => {
    jest.useRealTimers();
  });

  it('should return true for past date', () => {
    const pastDate = new Date('2024-03-15');
    expect(isExpired(pastDate)).toBe(true);
  });

  it('should return false for future date', () => {
    const futureDate = new Date('2024-03-17');
    expect(isExpired(futureDate)).toBe(false);
  });
});
```

---

## Test Data Management

### Test Fixtures

```javascript
// fixtures/users.js
export const validUser = {
  email: 'test@example.com',
  password: 'SecurePass123!',
  name: 'Test User'
};

export const adminUser = {
  email: 'admin@example.com',
  password: 'AdminPass123!',
  name: 'Admin User',
  role: 'admin'
};

// In tests
import { validUser } from '../fixtures/users';

it('should create user', async () => {
  const user = await createUser(validUser);
  expect(user.email).toBe(validUser.email);
});
```

### Factory Functions

```javascript
// factories/user.factory.js
let userCounter = 0;

export function createUserData(overrides = {}) {
  userCounter++;
  return {
    email: `user${userCounter}@example.com`,
    name: `User ${userCounter}`,
    password: 'Password123!',
    ...overrides
  };
}

// In tests
it('should handle multiple users', () => {
  const user1 = createUserData();
  const user2 = createUserData();
  const admin = createUserData({ role: 'admin' });

  // Each has unique email automatically
});
```

### Database Seeding

```javascript
// tests/helpers/seed.js
export async function seedDatabase() {
  await db.users.create({ email: 'test@example.com', name: 'Test' });
  await db.products.create({ name: 'Widget', price: 9.99 });
}

export async function clearDatabase() {
  await db.users.deleteMany({});
  await db.products.deleteMany({});
}

// In tests
beforeEach(async () => {
  await clearDatabase();
  await seedDatabase();
});
```

---

## Frontend Testing

### Component Testing (React)

```javascript
import { render, screen, fireEvent } from '@testing-library/react';
import UserProfile from './UserProfile';

describe('UserProfile', () => {
  const mockUser = {
    name: 'John Doe',
    email: 'john@example.com'
  };

  it('should render user information', () => {
    render(<UserProfile user={mockUser} />);

    expect(screen.getByText('John Doe')).toBeInTheDocument();
    expect(screen.getByText('john@example.com')).toBeInTheDocument();
  });

  it('should call onEdit when edit button is clicked', () => {
    const mockOnEdit = jest.fn();
    render(<UserProfile user={mockUser} onEdit={mockOnEdit} />);

    fireEvent.click(screen.getByText('Edit'));

    expect(mockOnEdit).toHaveBeenCalledWith(mockUser);
  });

  it('should show loading state', () => {
    render(<UserProfile user={null} loading={true} />);

    expect(screen.getByTestId('loading-spinner')).toBeInTheDocument();
  });
});
```

### Testing User Interactions

```javascript
it('should submit form with user input', async () => {
  const mockOnSubmit = jest.fn();
  render(<LoginForm onSubmit={mockOnSubmit} />);

  // Fill in form
  await userEvent.type(screen.getByLabelText('Email'), 'test@example.com');
  await userEvent.type(screen.getByLabelText('Password'), 'password123');

  // Submit
  await userEvent.click(screen.getByRole('button', { name: 'Log In' }));

  // Verify
  expect(mockOnSubmit).toHaveBeenCalledWith({
    email: 'test@example.com',
    password: 'password123'
  });
});
```

### Testing Async Behavior

```javascript
it('should fetch and display users', async () => {
  // Mock API
  const mockUsers = [
    { id: 1, name: 'User 1' },
    { id: 2, name: 'User 2' }
  ];
  jest.spyOn(api, 'fetchUsers').mockResolvedValue(mockUsers);

  render(<UserList />);

  // Initially shows loading
  expect(screen.getByText('Loading...')).toBeInTheDocument();

  // Wait for users to load
  const user1 = await screen.findByText('User 1');
  const user2 = await screen.findByText('User 2');

  expect(user1).toBeInTheDocument();
  expect(user2).toBeInTheDocument();
});
```

---

## API Testing

### Request/Response Testing

```javascript
describe('POST /api/users', () => {
  it('should create user with valid data', async () => {
    const userData = {
      email: 'newuser@example.com',
      name: 'New User',
      password: 'Password123!'
    };

    const response = await request(app)
      .post('/api/users')
      .send(userData);

    expect(response.status).toBe(201);
    expect(response.body).toMatchObject({
      id: expect.any(String),
      email: userData.email,
      name: userData.name,
      createdAt: expect.any(String)
    });
    expect(response.body.password).toBeUndefined(); // Never return password
  });

  it('should return 400 for invalid email', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ email: 'invalid-email', name: 'User' });

    expect(response.status).toBe(400);
    expect(response.body.error).toContain('email');
  });

  it('should return 409 for duplicate email', async () => {
    await createUser({ email: 'existing@example.com' });

    const response = await request(app)
      .post('/api/users')
      .send({ email: 'existing@example.com', name: 'User' });

    expect(response.status).toBe(409);
  });
});
```

### Authentication Testing

```javascript
describe('Protected Endpoints', () => {
  let authToken;

  beforeEach(async () => {
    const user = await createUser();
    authToken = generateToken(user.id);
  });

  it('should allow access with valid token', async () => {
    const response = await request(app)
      .get('/api/protected')
      .set('Authorization', `Bearer ${authToken}`);

    expect(response.status).toBe(200);
  });

  it('should reject request without token', async () => {
    const response = await request(app).get('/api/protected');
    expect(response.status).toBe(401);
  });

  it('should reject request with invalid token', async () => {
    const response = await request(app)
      .get('/api/protected')
      .set('Authorization', 'Bearer invalid-token');

    expect(response.status).toBe(401);
  });
});
```

---

## Performance Testing

### Load Testing

```javascript
// Use tools like Artillery, k6, or JMeter
// artillery.yml
config:
  target: 'https://api.yourapp.com'
  phases:
    - duration: 60
      arrivalRate: 10  # 10 requests per second
    - duration: 120
      arrivalRate: 50  # Ramp up to 50 rps

scenarios:
  - name: "Get users"
    flow:
      - get:
          url: "/api/users"
```

### Benchmark Tests

```javascript
// Measure performance of critical functions
describe('Performance', () => {
  it('should process 10k records in under 1 second', () => {
    const records = generateTestRecords(10000);

    const startTime = Date.now();
    processRecords(records);
    const duration = Date.now() - startTime;

    expect(duration).toBeLessThan(1000);
  });
});
```

---

## Test Maintenance

### Keep Tests Fast

```javascript
// ✅ Fast - uses mock
it('should send email notification', () => {
  const emailService = { send: jest.fn() };
  notifyUser(user, emailService);
  expect(emailService.send).toHaveBeenCalled();
});

// ❌ Slow - sends real email
it('should send email notification', async () => {
  await notifyUser(user);
  const email = await checkInbox(user.email);
  expect(email).toBeDefined();
});
```

### Refactor Tests When Refactoring Code

When you refactor code, update tests accordingly:
- Remove tests for deleted code
- Update tests for changed behavior
- Add tests for new functionality

### Fix Flaky Tests Immediately

Flaky tests (randomly fail) are worse than no tests:
- [ ] Investigate root cause
- [ ] Fix or temporarily skip with `.skip`
- [ ] Create ticket to address
- [ ] Never ignore!

---

## Continuous Integration

### Run Tests on Every Commit

```yaml
# .github/workflows/test.yml
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm ci

      - name: Run linter
        run: npm run lint

      - name: Run unit tests
        run: npm run test:unit

      - name: Run integration tests
        run: npm run test:integration

      - name: Upload coverage
        uses: codecov/codecov-action@v3
        with:
          files: ./coverage/lcov.info
```

### Block PRs with Failing Tests

- Require all tests to pass before merge
- Enforce minimum coverage thresholds
- Run tests in CI, not just locally

---

## Testing Checklist

### Before Pushing Code

- [ ] All tests pass locally
- [ ] Added tests for new functionality
- [ ] Added tests for bug fixes
- [ ] Updated tests for changed behavior
- [ ] Removed tests for deleted code
- [ ] No skipped tests (unless documented)
- [ ] Coverage meets minimum threshold

### Code Review

- [ ] Tests are clear and readable
- [ ] Tests verify correct behavior
- [ ] Tests cover edge cases
- [ ] Tests are isolated (no dependencies between tests)
- [ ] Mocks are appropriate (not over-mocking)
- [ ] Test names are descriptive

---

## Tools & Frameworks

### JavaScript/TypeScript
- **Unit/Integration**: Jest, Vitest, Mocha
- **E2E**: Playwright, Cypress, Puppeteer
- **React**: React Testing Library
- **API**: Supertest

### Test Organization

```
project/
├── src/
│   ├── components/
│   │   ├── Button.tsx
│   │   └── Button.test.tsx          # Colocated with component
│   ├── services/
│   │   ├── UserService.ts
│   │   └── UserService.test.ts
│   └── utils/
│       ├── validation.ts
│       └── validation.test.ts
├── tests/
│   ├── integration/                  # Integration tests
│   │   ├── api/
│   │   │   └── users.test.ts
│   │   └── database/
│   │       └── queries.test.ts
│   ├── e2e/                          # E2E tests
│   │   ├── auth.test.ts
│   │   └── checkout.test.ts
│   ├── fixtures/                     # Test data
│   │   ├── users.ts
│   │   └── products.ts
│   └── helpers/                      # Test utilities
│       ├── setup.ts
│       └── db.ts
└── package.json
```

---

## Metrics We Track

- **Test coverage**: % of code covered by tests
- **Test execution time**: How long test suite takes
- **Flaky test rate**: % of tests that intermittently fail
- **Test maintenance burden**: Time spent fixing/updating tests

**Goals**:
- Unit tests complete in < 30 seconds
- Full suite (including integration) < 5 minutes
- Flaky test rate < 1%
- Coverage > 80% overall

---

## Resources

- [Jest Documentation](https://jestjs.io/)
- [Testing Library](https://testing-library.com/)
- [Playwright Documentation](https://playwright.dev/)
- Internal testing workshop: [Link]

---

**Remember**: Tests are an investment. Well-written tests save time and prevent bugs. Poorly-written tests waste time and provide false confidence. Write tests that you'd want to read and maintain.

**Last Updated**: [Date]
**Next Review**: Quarterly
