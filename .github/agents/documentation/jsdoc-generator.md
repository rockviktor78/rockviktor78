---
agent: documentation-writer
---

# JSDoc Generator Agent

## Purpose

Automatically generate or complete JSDoc documentation for functions, classes, and components.

## Detection Criteria

Scan for missing documentation:

### Missing @fileoverview

```bash
grep -L "@fileoverview" src/app/**/*.ts
```

### Missing Function Documentation

Look for public functions without JSDoc:

```typescript
// Missing JSDoc
async loadData(id: string): Promise<Data> {
  // ...
}
```

### Incomplete JSDoc

Look for JSDoc without required tags:

- Missing @param
- Missing @returns
- Missing @throws

## JSDoc Templates

### File Header Template

```typescript
/**
 * @fileoverview [Short description of file purpose]
 * @description [Detailed description if needed]
 * @module [module-path]
 */
```

**Example:**

```typescript
/**
 * @fileoverview Authentication service
 * @description Handles user authentication with Firebase including login, logout,
 * and session management.
 * @module core/services
 */
```

### Function Template

```typescript
/**
 * [Brief description - what the function does]
 * @param [paramName] - [Parameter description]
 * @returns [What is returned and when]
 * @throws {ErrorType} [When and why error is thrown]
 */
```

**Examples:**

**Simple function:**

```typescript
/**
 * Calculates the sum of two numbers
 * @param a - First number
 * @param b - Second number
 * @returns Sum of a and b
 */
function add(a: number, b: number): number {
  return a + b;
}
```

**Async function:**

```typescript
/**
 * Loads user data from Firestore
 * @param userId - Unique user identifier
 * @returns Promise resolving to user object
 * @throws {Error} When user not found or network error occurs
 */
async loadUser(userId: string): Promise<User> {
  const userDoc = await getDoc(doc(this.firestore, 'users', userId));
  if (!userDoc.exists()) {
    throw new Error(`User ${userId} not found`);
  }
  return userDoc.data() as User;
}
```

**Complex function:**

```typescript
/**
 * Updates channel member permissions with transaction
 * @param channelId - Channel identifier
 * @param userId - User identifier
 * @param permissions - New permission flags
 * @returns Promise resolving when update is complete
 * @throws {PermissionError} When current user lacks admin rights
 * @throws {NotFoundError} When channel or user doesn't exist
 */
async updateMemberPermissions(
  channelId: string,
  userId: string,
  permissions: Permission[]
): Promise<void> {
  // Implementation
}
```

### Class/Component Template

```typescript
/**
 * @fileoverview [ComponentName] component
 * @description [What the component does and displays]
 * @module features/[feature-name]
 */

/**
 * [ComponentName]
 * @description [Detailed component purpose]
 */
@Component({...})
export class [ComponentName] {
  // ...
}
```

**Example:**

```typescript
/**
 * @fileoverview User profile component
 * @description Displays and manages user profile information including
 * avatar, name, email, and settings.
 * @module features/users
 */

/**
 * UserProfileComponent
 * @description Handles user profile display and editing with real-time
 * Firestore synchronization.
 */
@Component({
  selector: "app-user-profile",
  templateUrl: "./user-profile.component.html",
  styleUrl: "./user-profile.component.scss",
})
export class UserProfileComponent {
  // ...
}
```

### Service Template

```typescript
/**
 * @fileoverview [ServiceName] service
 * @description [Service purpose and responsibilities]
 * @module core/services
 */

/**
 * [ServiceName]
 * @description [Detailed service description]
 */
@Injectable({ providedIn: 'root' })
export class [ServiceName] {
  // ...
}
```

### Store Template

```typescript
/**
 * @fileoverview [Feature] store
 * @description State management for [feature] with Firebase integration
 * @module stores
 */

/**
 * [Feature]Store
 * @description Manages [feature] state with real-time Firestore listeners
 * and auto-cleanup on logout.
 */
export const [Feature]Store = signalStore(
  // ...
);
```

## Generation Rules

### 1. Analyze Function Signature

Extract from TypeScript:

- Parameter names and types
- Return type
- Async/Promise nature
- Throw statements

### 2. Generate Description

**Pattern:** `[Verb] [object] [context/purpose]`

Good examples:

- "Loads user data from Firestore"
- "Validates email format and uniqueness"
- "Toggles message reaction for current user"
- "Calculates thread reply count"

Bad examples:

- "Does stuff" (too vague)
- "User function" (unclear action)
- "Helper" (not descriptive)

### 3. Document Parameters

**Pattern:** `@param [name] - [What it represents and constraints]`

Examples:

```typescript
@param userId - Unique user identifier (UID from Firebase Auth)
@param email - User email address (must be validated format)
@param channels - Array of channel IDs to subscribe to (max 50)
@param options - Optional configuration with pagination settings
```

### 4. Document Returns

**Pattern:** `@returns [What is returned and when/conditions]`

Examples:

```typescript
@returns User object if found, null if not found
@returns Promise resolving to array of messages (empty if none)
@returns Unsubscribe function to stop listening
@returns Computed signal with formatted display name
```

### 5. Document Throws

**Pattern:** `@throws {ErrorType} [When and why]`

Examples:

```typescript
@throws {Error} When user not found in database
@throws {PermissionError} When user lacks required permissions
@throws {ValidationError} When email format is invalid
@throws {NetworkError} When Firestore connection fails
```

## Special Cases

### Signals & Computed

```typescript
/**
 * Current authenticated user
 * @type {Signal<User | null>}
 */
currentUser = signal<User | null>(null);

/**
 * User display name computed from user data
 * @type {Signal<string>}
 * @returns 'Guest' if no user, formatted name otherwise
 */
displayName = computed(() => {
  const user = this.currentUser();
  return user ? `${user.firstName} ${user.lastName}` : "Guest";
});
```

### Input/Output

```typescript
/**
 * User ID to display
 * @required
 */
userId = input.required<string>();

/**
 * Whether to show detailed information
 * @default false
 */
showDetails = input<boolean>(false);

/**
 * Emitted when user is selected
 * @event
 */
userSelected = output<User>();
```

### Getters/Setters

```typescript
/**
 * Gets the current user's role
 * @returns User role or 'guest' if not authenticated
 */
get userRole(): string {
  return this.currentUser()?.role || 'guest';
}

/**
 * Sets the active channel by ID
 * @param channelId - Channel identifier
 */
set activeChannel(channelId: string) {
  this.channelStore.setActive(channelId);
}
```

## Automation Checklist

- [ ] Find all `.ts` files without @fileoverview
- [ ] Find all public functions without JSDoc
- [ ] Find all JSDoc missing @param tags
- [ ] Find all JSDoc missing @returns tags
- [ ] Find all functions with `throw` but no @throws
- [ ] Generate appropriate documentation
- [ ] Verify generated docs match function behavior
- [ ] Check for consistency with existing docs

## Output Format

````markdown
## JSDoc Generation Report

### Files Missing @fileoverview: 12

1. `src/app/core/services/auth.service.ts`
   ```typescript
   /**
    * @fileoverview Authentication service
    * @description Manages user authentication with Firebase
    * @module core/services
    */
   ```
````

### Functions Missing JSDoc: 8

1. `src/app/features/chat/message.service.ts:45`
   ```typescript
   /**
    * Sends a message to a channel
    * @param channelId - Target channel identifier
    * @param content - Message text content
    * @returns Promise resolving when message is sent
    * @throws {Error} When channel not found or user not a member
    */
   async sendMessage(channelId: string, content: string): Promise<void>
   ```

### Incomplete JSDoc: 3

1. `src/app/stores/user.store.ts:78` - Missing @throws tag
   - Function throws Error but not documented
   - Suggested: `@throws {Error} When user update fails`

```

---

**Version:** 1.0
**Last Updated:** February 2026
**Project:** Join
```
