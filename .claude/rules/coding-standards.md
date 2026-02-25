# Coding Standards

These standards apply to all code Claude writes or modifies in projects using agent-coding-manager.

---

## 1. Architecture: Domain-Driven Design + SOLID

### Structure

Organize code by **domain** (what it does), not by **type** (what it is).

```
# Good — grouped by domain
src/
  users/
    user.service.ts
    user.repository.ts
    user.controller.ts
  orders/
    order.service.ts
    order.repository.ts

# Avoid — grouped by type
src/
  services/
  repositories/
  controllers/
```

### SOLID Summary

- **S** — Single Responsibility: each class/module has one reason to change.
- **O** — Open/Closed: extend behavior without modifying existing code.
- **L** — Liskov Substitution: subtypes must be usable in place of their base types.
- **I** — Interface Segregation: prefer small, focused interfaces over large ones.
- **D** — Dependency Inversion: depend on abstractions, not concrete implementations.

### When to adapt

- This is the default, not a rigid mandate — adapt to the project's existing patterns.
- If the project is functional (e.g., a Python project using functional style), follow that paradigm instead.

## 2. File & Class Header Comments

When a class or file plays a **key role in the project architecture** (e.g., a service, a repository, a controller, a core module, an entry point), add a short comment block at the top describing its responsibility in the project — not its internal logic.

- Keep it to 2-4 lines.
- Focus on **what role this file/class plays** and **what part of the system it serves**.
- Do not describe implementation details — just the architectural responsibility.

```ts
/**
 * Handles all user authentication flows: login, registration, and token refresh.
 * Acts as the entry point for auth-related API requests in the users domain.
 */
export class AuthController { ... }
```

```py
# Orchestrates order processing: validation, payment, and fulfillment.
# Other services call this — it's the main entry point for the orders domain.
class OrderService:
    ...
```

- Not every file needs this — utility helpers, small DTOs, config files, etc. can skip it.
- When working on a file that should have one and doesn't, add it.

## 3. Function Comments & Inline Explanations

### Every function gets a one-liner

- Every function or method must have a **short one-line comment** above it describing its purpose.
- Keep it concise — the "what" and "why", not the "how".

```
// Validates user input before saving to the database
function validateUserInput(data) { ... }
```

### Complex logic gets thought-chain comments

- When a function contains complex, non-obvious, or multi-step logic, add **inline comments that explain the reasoning chain** — step by step, like a thought process.
- The goal is that a reader can follow *why* each step happens, not just *what* it does.
- Keep comments close to the code they explain. Walk through the logic as if narrating it to another developer.

```
function calculateDiscount(order) {
  // Start with the base price — no discounts applied yet
  let price = order.basePrice;

  // Loyalty customers get 10% off, but only if they've been active for 6+ months
  // We check months instead of days to avoid edge cases around leap years
  if (order.customer.isLoyal && order.customer.activeMonths >= 6) {
    price *= 0.9;
  }

  // Bulk orders (50+ items) get an additional flat discount
  // This stacks with loyalty because we want to reward both behaviors
  if (order.itemCount >= 50) {
    price -= 25;
  }

  // Never go below zero — free orders break the invoicing system downstream
  return Math.max(price, 0);
}
```

### What NOT to comment

- Do not add redundant comments that just restate the code.
- Comments explain **why**, not **what** — the code already shows the "what".

### Fix old code that doesn't follow these rules

- When working on a file and you encounter existing code **that you are modifying or directly working with**, bring its comments up to these standards.
- Add the one-liner above functions that are missing it.
- Add thought-chain comments to complex logic that has none.
- **Only fix code you are already touching** — do not go on a commenting spree across unrelated files.

## 4. Function Size

- Each function should have a **clear, single purpose**.
- Don't write gigantic functions — break logic into clear, focused steps.
- But don't over-split either — keep related logic together so the reader has context.
- A function should do **one thing well** while remaining readable without jumping across 10 files.
- If a function exceeds ~40 lines, consider whether it can be broken down.

## 5. Error Handling

- Errors are a normal part of code. **Every operation that can fail must have its error handled.**
- Do not ignore, swallow, or silently discard errors.
- Handle errors at the appropriate level — where you have enough context to do something meaningful (log, retry, return a useful message, recover gracefully).
- Use the language's idiomatic error handling:
  - **Go**: always check returned `error` values — never use `_` to discard them.
  - **Rust**: use `Result`/`Option` properly — don't `.unwrap()` in production code without justification.
  - **TypeScript/JavaScript**: use `try/catch` around async operations, handle promise rejections.
  - **Python**: use `try/except` with specific exception types — never bare `except:`.
  - **C#/Java**: catch specific exceptions — never empty `catch {}` blocks.
- If you genuinely cannot handle an error locally, propagate it up clearly — don't hide it.
- External boundaries (network calls, file I/O, database queries, user input) **always** need error handling.

## 6. Null Safety

- Treat `null`, `nil`, `None`, `undefined`, and equivalent values as potential sources of bugs. **Analyze every nullable value before using it.**
- Before accessing properties, calling methods, or passing values downstream, verify they are not null/nil/undefined.
- Use the language's null-safety features:
  - **TypeScript**: enable `strictNullChecks`, use optional chaining (`?.`), nullish coalescing (`??`), and type guards.
  - **Kotlin**: use `?` types, `?.` safe calls, and `?:` elvis operator — avoid `!!` unless you can prove non-null.
  - **Swift**: use optionals properly — unwrap with `guard let` or `if let`, avoid force-unwrap (`!`) in production code.
  - **C#**: use nullable reference types, `?.`, `??`, and null checks.
  - **Rust**: use `Option<T>` — handle `None` explicitly, don't just `.unwrap()`.
  - **Go**: check for `nil` before dereferencing pointers or using interface values.
  - **Python**: check for `None` explicitly — don't assume a return value exists.
  - **Java**: use `Optional<T>` where appropriate, null-check return values from external code.
- Function return types should make nullability explicit — if a function can return nothing, the type signature must say so.
- When receiving data from external sources (APIs, databases, user input, deserialization), **always** assume fields could be null/missing and validate before use.

## 7. General

- Search for existing code before writing new code.
- Match the project's existing style and conventions.
- Keep changes minimal and focused on the task.
