# TypeScript Conventions

TypeScript-specific conventions for Node.js projects.

## Type System

### Enable Strict Mode
```json
// tsconfig.json
{
  "compilerOptions": {
    "strict": true,
    "noUncheckedIndexedAccess": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true
  }
}
```

### Avoid `any`
```typescript
// Never do this
function process(data: any): any {
  return data.value;
}

// Use unknown for truly unknown types
function process(data: unknown): string {
  if (typeof data === 'object' && data !== null && 'value' in data) {
    return String(data.value);
  }
  throw new Error('Invalid data format');
}

// Or define proper interfaces
interface DataInput {
  value: string;
}

function process(data: DataInput): string {
  return data.value;
}
```

### Define Interfaces for Data
```typescript
// Define shapes for all data structures
interface User {
  id: string;
  email: string;
  name: string;
  createdAt: Date;
  updatedAt: Date;
}

interface CreateUserInput {
  email: string;
  name: string;
}

// Use Pick/Omit for variations
type UpdateUserInput = Partial<Omit<User, 'id' | 'createdAt'>>;
```

### Discriminated Unions for State
```typescript
// Good: type-safe state handling
type RequestState<T> =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success'; data: T }
  | { status: 'error'; error: Error };

function handleState(state: RequestState<User>) {
  switch (state.status) {
    case 'idle':
      return 'Ready to load';
    case 'loading':
      return 'Loading...';
    case 'success':
      return `Hello, ${state.data.name}`;  // TypeScript knows data exists
    case 'error':
      return `Error: ${state.error.message}`;  // TypeScript knows error exists
  }
}
```

## Patterns

### Extract Event Handlers
```typescript
// Don't: inline handlers
button.addEventListener('click', (e) => {
  // lots of logic here
});

// Do: named functions for testability
function handleButtonClick(e: MouseEvent) {
  // logic here
}
button.addEventListener('click', handleButtonClick);
```

### Async/Await Over Callbacks
```typescript
// Don't use callbacks
function fetchUser(id: string, callback: (err: Error | null, user?: User) => void) {
  // ...
}

// Use async/await
async function fetchUser(id: string): Promise<User> {
  const response = await fetch(`/api/users/${id}`);
  if (!response.ok) {
    throw new Error(`Failed to fetch user: ${response.status}`);
  }
  return response.json();
}
```

### Error Handling
```typescript
// Define domain errors
class DomainError extends Error {
  constructor(message: string) {
    super(message);
    this.name = 'DomainError';
  }
}

class UserNotFoundError extends DomainError {
  constructor(public userId: string) {
    super(`User not found: ${userId}`);
    this.name = 'UserNotFoundError';
  }
}

// Use Result type for expected failures
type Result<T, E = Error> =
  | { ok: true; value: T }
  | { ok: false; error: E };

async function findUser(id: string): Promise<Result<User, UserNotFoundError>> {
  const user = await db.users.findById(id);
  if (!user) {
    return { ok: false, error: new UserNotFoundError(id) };
  }
  return { ok: true, value: user };
}
```

## API Clients

### Separate Server/Client Code
```typescript
// server/api.ts - runs on server only
import { db } from './db';

export async function getUser(id: string): Promise<User> {
  return db.users.findById(id);
}

// client/api.ts - runs in browser
export async function getUser(id: string): Promise<User> {
  const res = await fetch(`/api/users/${id}`);
  return res.json();
}
```

### Type API Responses
```typescript
// Define response types
interface ApiResponse<T> {
  data: T;
  meta?: {
    page: number;
    total: number;
  };
}

interface ApiError {
  code: string;
  message: string;
}

// Use Zod for runtime validation
import { z } from 'zod';

const UserSchema = z.object({
  id: z.string(),
  email: z.string().email(),
  name: z.string(),
});

async function fetchUser(id: string): Promise<User> {
  const response = await fetch(`/api/users/${id}`);
  const json = await response.json();
  return UserSchema.parse(json);  // Runtime validation
}
```

## Project Structure

```
project/
├── src/
│   ├── index.ts          # Entry point
│   ├── config.ts         # Configuration
│   ├── types/            # Type definitions
│   │   └── index.ts
│   ├── models/           # Data models
│   ├── services/         # Business logic
│   ├── api/              # API routes/handlers
│   └── utils/            # Shared utilities
├── tests/
│   ├── setup.ts          # Test configuration
│   ├── unit/
│   └── integration/
├── package.json
├── tsconfig.json
└── README.md
```

## Tools

### ESLint Configuration
```json
{
  "extends": [
    "eslint:recommended",
    "@typescript-eslint/recommended",
    "@typescript-eslint/recommended-requiring-type-checking"
  ],
  "rules": {
    "@typescript-eslint/no-explicit-any": "error",
    "@typescript-eslint/explicit-function-return-type": "warn"
  }
}
```

### Scripts
```json
{
  "scripts": {
    "build": "tsc",
    "lint": "eslint src/",
    "test": "vitest",
    "typecheck": "tsc --noEmit"
  }
}
```

### Running Checks
```bash
# Type check
npm run typecheck

# Lint
npm run lint

# Test
npm test

# All checks
npm run typecheck && npm run lint && npm test
```
