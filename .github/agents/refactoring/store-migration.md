---
agent: refactoring-assistant
---

# Store Migration Agent

## Purpose

Migrate localStorage-based dummy services to Firebase-integrated SignalStores with proper cleanup patterns.

## Migration Pattern

### Phase 1: Identify Target Service

Look for services matching these patterns:

- `dummy-*.service.ts`
- `current-*.service.ts`
- Services using `localStorage.getItem()` or `localStorage.setItem()`
- Services in `features/dashboard/services/` folder

**Example targets:**

```
src/app/features/dashboard/services/
├── dummy-channels.service.ts
├── dummy-chat-dm.service.ts
├── dummy-mailbox.service.ts
├── dummy-thread.service.ts
└── dummy-users.service.ts
```

### Phase 2: Create Store Structure

For simple services (< 100 LOC total):

```
src/app/stores/
└── [feature].store.ts
```

For complex services (> 100 LOC):

```
src/app/stores/[feature]/
├── [feature].store.ts
├── [feature].types.ts
├── [feature].helpers.ts
├── [feature].methods.ts
└── index.ts
```

### Phase 3: Implement Store

**Template for simple store:**

```typescript
import { signalStore, withState, withMethods, withComputed } from '@ngrx/signals';
import { inject, computed } from '@angular/core';
import { Firestore, collection, onSnapshot, addDoc, deleteDoc, doc } from '@angular/fire/firestore';

interface [Feature]State {
  items: [Type][];
  isLoading: boolean;
  error: string | null;
  unsubscribe: (() => void) | null;
}

const initialState: [Feature]State = {
  items: [],
  isLoading: false,
  error: null,
  unsubscribe: null,
};

export const [Feature]Store = signalStore(
  { providedIn: 'root' },

  withState(initialState),

  withComputed((store) => ({
    // Add computed properties
  })),

  withMethods((store) => {
    const firestore = inject(Firestore);

    return {
      /**
       * Load items from Firestore
       */
      load(): void {
        // Cleanup previous listener
        const unsub = store.unsubscribe();
        if (unsub) unsub();

        store.isLoading.set(true);
        const itemsRef = collection(firestore, '[collection]');

        const unsubscribe = onSnapshot(itemsRef,
          (snapshot) => {
            const items = snapshot.docs.map(doc => ({
              id: doc.id,
              ...doc.data()
            } as [Type]));

            store.items.set(items);
            store.isLoading.set(false);
          },
          (error) => {
            // Handle permission errors on logout
            if (error.code === 'permission-denied') {
              this.cleanup();
            } else {
              store.error.set(error.message);
              store.isLoading.set(false);
            }
          }
        );

        store.unsubscribe.set(unsubscribe);
      },

      /**
       * Add new item
       */
      async add(item: Partial<[Type]>): Promise<void> {
        const itemsRef = collection(firestore, '[collection]');
        await addDoc(itemsRef, item);
      },

      /**
       * Delete item
       */
      async delete(id: string): Promise<void> {
        const itemRef = doc(firestore, '[collection]', id);
        await deleteDoc(itemRef);
      },

      /**
       * Cleanup on logout
       */
      cleanup(): void {
        const unsub = store.unsubscribe();
        if (unsub) unsub();

        store.items.set([]);
        store.isLoading.set(false);
        store.error.set(null);
        store.unsubscribe.set(null);
      },
    };
  })
);

// Type export
export type { [Feature]State };
```

### Phase 4: Update Components

**Before (using service):**

```typescript
export class MyComponent {
  private dummyService = inject(DummyService);

  items = signal<Item[]>([]);

  ngOnInit() {
    this.items.set(this.dummyService.getItems());
  }

  addItem(item: Item) {
    this.dummyService.addItem(item);
    this.items.set(this.dummyService.getItems());
  }
}
```

**After (using store):**

```typescript
export class MyComponent {
  private itemStore = inject(ItemStore);

  items = this.itemStore.items;
  isLoading = this.itemStore.isLoading;

  constructor() {
    this.itemStore.load();
  }

  async addItem(item: Item) {
    await this.itemStore.add(item);
  }
}
```

### Phase 5: Remove Old Service

1. Verify all components updated
2. Search for service imports: `grep -r "DummyService" src/`
3. Delete service file
4. Remove from any module providers (if applicable)

## Migration Checklist

### Store Implementation

- [ ] State interface defined with proper types
- [ ] Initial state created
- [ ] Firestore collection reference correct
- [ ] Real-time listener with onSnapshot()
- [ ] Permission error handling (`permission-denied`)
- [ ] Auto-cleanup implemented
- [ ] unsubscribe stored and called
- [ ] Type exports added (`export type`)

### Data Migration

- [ ] Firestore collection structure matches model
- [ ] Security rules updated for new collection
- [ ] Indexes created if needed (composite queries)
- [ ] Initial data seeded if required

### Component Updates

- [ ] All components using service identified
- [ ] inject(Store) replaces inject(Service)
- [ ] Direct signal references (no .set() needed)
- [ ] load() called in constructor or ngOnInit
- [ ] async methods awaited properly
- [ ] cleanup() called on logout (if applicable)

### Testing

- [ ] Store loads data correctly
- [ ] Real-time updates work
- [ ] Add/update/delete operations work
- [ ] Cleanup prevents errors on logout
- [ ] No console errors
- [ ] No memory leaks

### Cleanup

- [ ] Old service file deleted
- [ ] All imports removed
- [ ] No references in codebase
- [ ] README updated if service was documented

## Common Patterns

### Pattern 1: Simple CRUD Store

```typescript
// For channels, users, messages
BasicStore: load(), add(), update(), delete(), cleanup()
```

### Pattern 2: Filtered Store

```typescript
// For user-specific data (DMs, threads)
FilteredStore: load(userId), filtered computed signal
```

### Pattern 3: Nested Collection Store

```typescript
// For subcollections (channel messages, thread replies)
NestedStore: load(parentId, childCollection);
```

### Pattern 4: Relationship Store

```typescript
// For many-to-many (channel members, invitations)
RelationshipStore: (addRelation(), removeRelation(), getRelated());
```

## Example Migration: DummyThreadService → ThreadStore

**Before:**

```typescript
// dummy-thread.service.ts
@Injectable({ providedIn: "root" })
export class DummyThreadService {
  private threads = signal<Thread[]>([]);

  getThreads(): Thread[] {
    const stored = localStorage.getItem("threads");
    return stored ? JSON.parse(stored) : [];
  }

  addThread(thread: Thread): void {
    const threads = this.getThreads();
    threads.push(thread);
    localStorage.setItem("threads", JSON.stringify(threads));
  }
}
```

**After:**

```typescript
// thread.store.ts
export const ThreadStore = signalStore(
  { providedIn: "root" },
  withState({ threads: [], unsubscribe: null }),
  withMethods((store) => {
    const firestore = inject(Firestore);
    return {
      loadThreads(channelId: string, messageId: string): void {
        const threadsRef = collection(
          firestore,
          `channels/${channelId}/messages/${messageId}/replies`,
        );

        const unsubscribe = onSnapshot(threadsRef, (snapshot) => {
          store.threads.set(
            snapshot.docs.map((doc) => ({
              id: doc.id,
              ...doc.data(),
            })),
          );
        });

        store.unsubscribe.set(unsubscribe);
      },

      cleanup(): void {
        store.unsubscribe()?.();
        store.threads.set([]);
      },
    };
  }),
);
```

---

**Version:** 1.0
**Last Updated:** February 2026
**Project:** DABubble
