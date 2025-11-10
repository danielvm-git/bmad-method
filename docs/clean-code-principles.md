# Clean Code Principles

## Introduction

Clean Code is a set of industry best practices for writing maintainable, readable, and robust software. These principles are based on Robert C. Martin's seminal book _Clean Code: A Handbook of Agile Software Craftsmanship_ (2008) and decades of software engineering wisdom.

**Why Clean Code Matters:**

- **Maintainability**: Code is read 10x more than it's written. Clean code reduces cognitive load.
- **Bug Prevention**: Clear, simple code has fewer places for bugs to hide.
- **Team Velocity**: Teams move faster when they can understand and modify code quickly.
- **Professional Excellence**: Clean code is a mark of software craftsmanship.

**Guiding Principle**: Leave code better than you found it ("Boy Scout Rule").

---

## Core Principles

### 1. Meaningful Names

**Principle**: Names should reveal intent. A reader should understand what a variable, function, or class does without reading its implementation.

**Guidelines**:

- Use intention-revealing names
- Avoid mental mapping (single-letter variables except loop counters)
- Use searchable names (no magic numbers)
- Class names: nouns or noun phrases (`User`, `AccountManager`)
- Function names: verbs or verb phrases (`calculateTotal`, `isValid`)
- Be consistent with terminology

**❌ Bad Example**:

```javascript
function getThem(list) {
  const list1 = [];
  for (const x of list) {
    if (x[0] == 4) {
      list1.push(x);
    }
  }
  return list1;
}
```

**✅ Good Example**:

```javascript
function getFlaggedCells(gameBoard) {
  const flaggedCells = [];
  for (const cell of gameBoard) {
    if (cell.isFlagged()) {
      flaggedCells.push(cell);
    }
  }
  return flaggedCells;
}
```

**❌ Bad Example (Python)**:

```python
def process(d):
    t = 0
    for i in d:
        t += i * 0.21
    return t
```

**✅ Good Example (Python)**:

```python
TAX_RATE = 0.21

def calculate_total_tax(prices):
    total_tax = 0
    for price in prices:
        total_tax += price * TAX_RATE
    return total_tax
```

---

### 2. Small Functions

**Principle**: Functions should be small—ideally under 20 lines. They should do ONE thing, do it well, and do it only.

**Guidelines**:

- **<20 lines is a guideline**, not a hard rule
- Suggest refactoring if exceeded, but don't block implementation
- Exception: Framework patterns (React hooks, Django views, setup functions)
- Business logic must stay focused
- Extract 'til you drop: if you can extract a function, you probably should

**Function Size Hierarchy**:

1. **Ideal**: 1-10 lines
2. **Good**: 11-20 lines
3. **Acceptable**: 21-30 lines (with justification)
4. **Refactor**: 30+ lines (unless framework pattern)

**❌ Bad Example**:

```javascript
function processOrder(order) {
  // Validate order (5 lines)
  if (!order.items || order.items.length === 0) {
    throw new Error('Order has no items');
  }
  if (!order.customer || !order.customer.email) {
    throw new Error('Invalid customer');
  }

  // Calculate totals (8 lines)
  let subtotal = 0;
  for (const item of order.items) {
    subtotal += item.price * item.quantity;
  }
  const tax = subtotal * 0.21;
  const shipping = subtotal > 100 ? 0 : 10;
  const total = subtotal + tax + shipping;

  // Save to database (5 lines)
  const savedOrder = db.orders.create({
    ...order,
    subtotal,
    tax,
    shipping,
    total,
    status: 'pending',
  });

  // Send email (5 lines)
  sendEmail(order.customer.email, {
    subject: 'Order Confirmation',
    body: `Your order #${savedOrder.id} total: $${total}`,
  });

  return savedOrder;
}
// 30+ lines - does validation, calculation, persistence, notification
```

**✅ Good Example**:

```javascript
function processOrder(order) {
  validateOrder(order);
  const totals = calculateOrderTotals(order);
  const savedOrder = saveOrder(order, totals);
  notifyCustomer(savedOrder);
  return savedOrder;
}
// 6 lines - clear flow, delegates to focused functions

function validateOrder(order) {
  if (!order.items || order.items.length === 0) {
    throw new Error('Order has no items');
  }
  if (!order.customer || !order.customer.email) {
    throw new Error('Invalid customer');
  }
}

function calculateOrderTotals(order) {
  const subtotal = order.items.reduce((sum, item) => sum + item.price * item.quantity, 0);
  const tax = subtotal * TAX_RATE;
  const shipping = subtotal > FREE_SHIPPING_THRESHOLD ? 0 : SHIPPING_COST;
  return { subtotal, tax, shipping, total: subtotal + tax + shipping };
}

function saveOrder(order, totals) {
  return db.orders.create({
    ...order,
    ...totals,
    status: 'pending',
  });
}

function notifyCustomer(order) {
  sendEmail(order.customer.email, {
    subject: 'Order Confirmation',
    body: `Your order #${order.id} total: $${order.total}`,
  });
}
```

---

### 3. Single Responsibility Principle (SRP)

**Principle**: A class or function should have one, and only one, reason to change. Each unit of code should have a single, well-defined responsibility.

**Guidelines**:

- One responsibility per function/class/module
- If you use "and" to describe what it does, it probably does too much
- Changes to one aspect shouldn't affect unrelated code
- Promotes reusability and testability

**❌ Bad Example**:

```python
class UserManager:
    def create_user(self, username, email, password):
        # Validation
        if not email or '@' not in email:
            raise ValueError('Invalid email')
        if len(password) < 8:
            raise ValueError('Password too short')

        # Hash password
        hashed = hashlib.sha256(password.encode()).hexdigest()

        # Save to database
        user = db.users.insert({
            'username': username,
            'email': email,
            'password': hashed,
            'created_at': datetime.now()
        })

        # Send welcome email
        smtp = smtplib.SMTP('smtp.example.com')
        msg = f'Welcome {username}!'
        smtp.sendmail('noreply@example.com', email, msg)

        # Log activity
        with open('users.log', 'a') as f:
            f.write(f'{datetime.now()}: User {username} created\n')

        return user
# Violates SRP: validation, hashing, persistence, email, logging
```

**✅ Good Example**:

```python
class UserValidator:
    def validate(self, email, password):
        if not email or '@' not in email:
            raise ValueError('Invalid email')
        if len(password) < 8:
            raise ValueError('Password too short')

class PasswordHasher:
    def hash(self, password):
        return hashlib.sha256(password.encode()).hexdigest()

class UserRepository:
    def save(self, username, email, hashed_password):
        return db.users.insert({
            'username': username,
            'email': email,
            'password': hashed_password,
            'created_at': datetime.now()
        })

class UserNotifier:
    def send_welcome_email(self, email, username):
        smtp = smtplib.SMTP('smtp.example.com')
        msg = f'Welcome {username}!'
        smtp.sendmail('noreply@example.com', email, msg)

class UserActivityLogger:
    def log_user_created(self, username):
        with open('users.log', 'a') as f:
            f.write(f'{datetime.now()}: User {username} created\n')

class UserService:
    def __init__(self, validator, hasher, repository, notifier, logger):
        self.validator = validator
        self.hasher = hasher
        self.repository = repository
        self.notifier = notifier
        self.logger = logger

    def create_user(self, username, email, password):
        self.validator.validate(email, password)
        hashed = self.hasher.hash(password)
        user = self.repository.save(username, email, hashed)
        self.notifier.send_welcome_email(email, username)
        self.logger.log_user_created(username)
        return user
# Each class has ONE reason to change
```

---

### 4. Don't Repeat Yourself (DRY)

**Principle**: Every piece of knowledge should have a single, authoritative representation in the system. Duplication is waste and a source of bugs.

**Guidelines**:

- Extract repeated logic into functions
- Use constants for repeated values
- Create abstractions for similar patterns
- But: don't abstract prematurely—wait for 3rd occurrence ("Rule of Three")

**❌ Bad Example**:

```javascript
function calculateShippingForUSA(weight) {
  if (weight < 1) return 5.99;
  if (weight < 5) return 9.99;
  if (weight < 10) return 14.99;
  return 19.99;
}

function calculateShippingForCanada(weight) {
  if (weight < 1) return 7.99;
  if (weight < 5) return 12.99;
  if (weight < 10) return 18.99;
  return 24.99;
}

function calculateShippingForMexico(weight) {
  if (weight < 1) return 6.99;
  if (weight < 5) return 10.99;
  if (weight < 10) return 16.99;
  return 21.99;
}
// Same logic duplicated 3 times
```

**✅ Good Example**:

```javascript
const SHIPPING_RATES = {
  USA: [5.99, 9.99, 14.99, 19.99],
  CANADA: [7.99, 12.99, 18.99, 24.99],
  MEXICO: [6.99, 10.99, 16.99, 21.99],
};

const WEIGHT_BRACKETS = [1, 5, 10];

function calculateShipping(country, weight) {
  const rates = SHIPPING_RATES[country];
  if (!rates) throw new Error(`Unknown country: ${country}`);

  for (let i = 0; i < WEIGHT_BRACKETS.length; i++) {
    if (weight < WEIGHT_BRACKETS[i]) {
      return rates[i];
    }
  }
  return rates[rates.length - 1];
}
// Single implementation, data-driven
```

---

### 5. Error Handling

**Principle**: Error handling is important, but it shouldn't obscure logic. Use exceptions, fail fast, and provide meaningful error messages.

**Guidelines**:

- Use exceptions, not error codes
- Provide context with exceptions
- Don't return null; throw exceptions or use Option/Maybe types
- Don't ignore caught exceptions
- Fail fast at boundaries (validate early)
- Write error messages for humans

**❌ Bad Example**:

```javascript
function getUserById(id) {
  try {
    const user = db.users.findOne(id);
    if (user) {
      return user;
    } else {
      return null; // Swallowing error
    }
  } catch (e) {
    console.log(e); // Just logging, not handling
    return null;
  }
}

function displayUser(id) {
  const user = getUserById(id);
  console.log(user.name); // Potential null reference error!
}
```

**✅ Good Example**:

```javascript
class UserNotFoundError extends Error {
  constructor(id) {
    super(`User not found with ID: ${id}`);
    this.name = 'UserNotFoundError';
    this.userId = id;
  }
}

function getUserById(id) {
  if (!id) {
    throw new Error('User ID is required');
  }

  try {
    const user = db.users.findOne(id);
    if (!user) {
      throw new UserNotFoundError(id);
    }
    return user;
  } catch (error) {
    if (error instanceof UserNotFoundError) {
      throw error; // Re-throw domain errors
    }
    // Wrap infrastructure errors with context
    throw new Error(`Failed to retrieve user ${id}: ${error.message}`);
  }
}

function displayUser(id) {
  try {
    const user = getUserById(id);
    console.log(user.name);
  } catch (error) {
    if (error instanceof UserNotFoundError) {
      console.error('User not found. Please check the ID.');
    } else {
      console.error('An error occurred:', error.message);
    }
  }
}
```

**❌ Bad Example (Python)**:

```python
def divide(a, b):
    if b == 0:
        return -1  # Error code
    return a / b

result = divide(10, 0)
if result == -1:  # Caller must remember to check
    print('Error')
```

**✅ Good Example (Python)**:

```python
def divide(a, b):
    if b == 0:
        raise ValueError('Cannot divide by zero')
    return a / b

try:
    result = divide(10, 0)
except ValueError as e:
    print(f'Error: {e}')
```

---

### 6. F.I.R.S.T. Test Principles

**Principle**: Tests should be **F**ast, **I**ndependent, **R**epeatable, **S**elf-validating, and **T**imely.

**Guidelines**:

#### Fast

- Unit tests: <100ms each
- Integration tests: <5s each
- Slow tests don't get run, so they're useless

#### Independent

- Tests can run in any order
- No shared state between tests
- Each test sets up its own data

#### Repeatable

- Same result every time
- No flaky tests
- No environment dependencies (time, random, network)

#### Self-Validating

- Pass or fail, no manual inspection
- Clear assertions
- Meaningful failure messages

#### Timely

- Write tests BEFORE or WITH code, not after
- TDD: Red → Green → Refactor

**❌ Bad Example**:

```javascript
// Slow, not independent, flaky
let globalUser;

test('create user', async () => {
  globalUser = await createUser('alice'); // Modifies global state
  expect(globalUser.name).toBe('alice');
  await new Promise((resolve) => setTimeout(resolve, 5000)); // Slow!
});

test('get user', async () => {
  const user = await getUserById(globalUser.id); // Depends on previous test
  expect(user.name).toBe('alice');
});

test('random test', () => {
  const value = Math.random(); // Not repeatable
  expect(value).toBeGreaterThan(0.5); // Flaky!
});
```

**✅ Good Example**:

```javascript
// Fast, independent, repeatable, self-validating, timely
describe('UserService', () => {
  let userService;
  let mockDb;

  beforeEach(() => {
    // Each test gets fresh state
    mockDb = createMockDatabase();
    userService = new UserService(mockDb);
  });

  test('createUser creates user with valid data', () => {
    const userData = { name: 'alice', email: 'alice@example.com' };
    const user = userService.createUser(userData);

    expect(user.name).toBe('alice');
    expect(user.email).toBe('alice@example.com');
    expect(mockDb.users).toHaveLength(1);
  }); // Fast (<10ms), independent, repeatable

  test('createUser throws error for invalid email', () => {
    const userData = { name: 'bob', email: 'invalid' };

    expect(() => userService.createUser(userData)).toThrow('Invalid email');
  }); // Self-validating with clear assertion

  test('getUserById returns user when exists', () => {
    const user = userService.createUser({ name: 'charlie', email: 'charlie@example.com' });
    const retrieved = userService.getUserById(user.id);

    expect(retrieved).toEqual(user);
  }); // Independent - creates its own data
});
```

---

## Framework Conventions Override

**Important**: Clean Code principles are **guidelines**, not absolute rules. When framework conventions conflict, **defer to the framework**.

**Examples of Acceptable Framework Patterns**:

### React Hooks (>20 lines)

```javascript
// ACCEPTABLE: React hooks with complex state logic
function useOrderManagement(initialOrders) {
  const [orders, setOrders] = useState(initialOrders);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    // Subscription logic
    const subscription = orderService.subscribe((update) => {
      setOrders((prev) => [...prev, update]);
    });
    return () => subscription.unsubscribe();
  }, []);

  const addOrder = useCallback(async (order) => {
    setLoading(true);
    try {
      const created = await orderService.create(order);
      setOrders((prev) => [...prev, created]);
      return created;
    } catch (err) {
      setError(err.message);
      throw err;
    } finally {
      setLoading(false);
    }
  }, []);

  return { orders, loading, error, addOrder };
}
// 30+ lines, but standard React pattern - ACCEPTABLE
```

### Django Views

```python
# ACCEPTABLE: Django class-based view
class UserListView(ListView):
    model = User
    template_name = 'users/list.html'
    context_object_name = 'users'
    paginate_by = 20

    def get_queryset(self):
        queryset = super().get_queryset()
        search = self.request.GET.get('search')
        if search:
            queryset = queryset.filter(
                Q(username__icontains=search) |
                Q(email__icontains=search)
            )
        return queryset.order_by('-created_at')

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['search_query'] = self.request.GET.get('search', '')
        context['total_users'] = User.objects.count()
        return context
# Django pattern - ACCEPTABLE
```

**When to Note in Reviews**:

- Document: "Framework pattern acceptable per React/Django conventions"
- Don't flag as violation
- Ensure business logic is still extracted and tested

---

## Language-Specific Considerations

### JavaScript/TypeScript

- Use `const` by default, `let` when needed, avoid `var`
- Prefer arrow functions for callbacks
- Use destructuring for clarity: `const { name, email } = user;`
- Async/await over promise chains
- Use optional chaining: `user?.profile?.avatar`

### Python

- Follow PEP 8 style guide
- Use list comprehensions for simple transformations
- Context managers (`with`) for resource handling
- Type hints for function signatures (Python 3.5+)
- Use `pathlib` for file paths, not string concatenation

### Go

- Embrace Go idioms (explicit error handling)
- Keep packages focused (one concept per package)
- Use interfaces for abstraction
- Prefer composition over inheritance
- Error handling: check errors, don't ignore

---

## References and Further Reading

**Books**:

- **Clean Code** by Robert C. Martin (2008) - The definitive guide
- **The Pragmatic Programmer** by Hunt & Thomas (2019) - Timeless wisdom
- **Refactoring** by Martin Fowler (2018) - How to improve existing code
- **Code Complete** by Steve McConnell (2004) - Comprehensive reference

**Online Resources**:

- [Clean Code JavaScript](https://github.com/ryanmcdermott/clean-code-javascript) - JS-specific guide
- [Clean Code Python](https://github.com/zedr/clean-code-python) - Python-specific guide
- [F.I.R.S.T. Principles](https://github.com/ghsukumar/SFDC_Best_Practices/wiki/F.I.R.S.T-Principles-of-Unit-Testing) - Test quality

**BMAD Resources**:

- [Naming Conventions Guide](./naming-conventions-guide.md) - BMAD naming standards
- [Conventional Commits Guide](./conventional-commits-guide.md) - Commit message format
- [Date Format Standard](./date-format-standard.md) - ISO 8601 dates

---

## Summary Checklist

When writing or reviewing code, ask:

- [ ] **Names**: Do names reveal intent without comments?
- [ ] **Functions**: Is each function <20 lines and doing ONE thing?
- [ ] **Responsibility**: Does each class/module have a single reason to change?
- [ ] **DRY**: Is there any duplicated logic to extract?
- [ ] **Errors**: Are exceptions used? Are error messages meaningful?
- [ ] **Tests**: Are tests fast, independent, repeatable, self-validating, and timely?
- [ ] **Framework**: Does it follow framework conventions where applicable?

**Remember**: Clean Code is about **communication**. Code is for humans first, computers second.

---

**Version**: 1.0
**Last Updated**: 2025-11-10
**Maintained by**: BMAD Development Team
