# Coroutine Scope Cancellation

## Scope Cancellation vs Scope's Children Cancellation

### To cancel a coroutine scope

```kotlin
scope.cancel()
```

OR

```kotlin
scope.coroutineContext.cancel()
```

- Once cancelled, can not be restarted.

### To cancel a coroutine scope's children

```kotlin
scope.coroutineContext.cancelChildren()
```

- Cancels all the children of the coroutine scope.
- The coroutine will be active even after cancelling children.

#### Recommendation

- Always cancel coroutine scope's children.

## Coroutine Scope Inside ViewModel

#### Recommendation

- Can use `viewModelScope` instead of manual coroutine scope handling.
- `viewModelScope` uses the main thread internally.
- `viewModelScope` will automatically handle the cancellation of the coroutines.
  It uses `CloseableCoroutineScope` internally and it ties the coroutine lifecycle with the ViewModel lifecycle.

## Coroutine Scope From Kotlin Extension For ViewModel

- Covered in above section.

# Structured Concurrency

- Ability to pause code execution and wait for all concurrent flows which can be traced back to a specific ancestor to complete.

# Design with Coroutines

## The Main Rule of Concurrency in Android

- The presentation layer logic should use the main/UI thread exclusively.
- No multi-threading.
- UI Logic, Android controllers (Activities, Fragments), Standalone controllers (Controllers, Presenters, ViewModels, etc.)

## Encapsulating concurrency in Use Cases

- Refactor concurrent code into use-cases in domain layer.

# Coroutine Dispatchers

## Main Dispatcher

### Main vs Main.immediate

- `Main.immediate` will start execution immediately if the current thread is Main thread. Otherwise it will behave similar to Main dispatcher.

#### Recommendation

- Use `Main.immediate` for most scenarios, unless `Main` is required.

## Background Dispatchers

- Dispatchers.Default and Dispatcher.IO.
- Default dispatcher is used for computation intensive tasks.
  - Number of threads - `max(2, NUM_OF_CPU_CORES)`.
- IO dispatcher is used for input output tasks which wait for a long time.
  - Number of threads - `max(64, NUM_OF_CPU_CORES)`.
- Kotlin coroutines suspend and do not block the thread, but non coroutines code might block the thread.

## Unconfined Dispatcher

- Uses the same thread that is starting the coroutine. (Same dispatcher).
- Unconfined executes similar to `Main.immediate` and might execute the code immediately.

#### Recommendation

- No probable use-cases.

# Coroutine Cancellation

## Cooperative Cancellation

- `isActive`
- Catch `CancellationException` to handle when a coroutine cancels. E.g. Clean up work.
- Special suspend functions like `delay()`, `withContext()`, etc check if the coroutine is active when starting the code execution and before returning the result, and hence they throw `CancellationException` if the coroutine is cancelled.

## The Importance of Cancellation Exception

- `ensureActive()`
- To throw `CancellationException` once the coroutine is cancelled.

## NonCancellable

- `NonCancellable` - Special Job to ensure that a coroutine code is executed completely even if the coroutine is cancelled.

# Coroutines Mechanics

## CoroutineScope and CoroutineContext

### CoroutineScope

- Coroutine scope is a wrapper for a coroutine context. It is an interface containing a single property for the corotine context.

- To access context

  ```kotlin
  this.coroutineContext
  ```

- To access the Job

  ```kotlin
  this.coroutineContext[Job]
  ```

### CoroutineContext

- `EmptyCoroutineContext`

## Coroutine Context Elements

- `CoroutineName` - A name for a coroutine.
- `CoroutineContext` can have an id, name, Job, Dispatcher, etc.

### CoroutineDispatcher

### Job

## withContext Function

- A suspend method that allows to change context, that internally uses `suspendCoroutineUninterceptedOrReturn()`.
- This does not create a new coroutine.

## Jobs Hierarchy

- Every coorutine has a corresponding Job, but not every Job has a corresponding coroutine.
  Example - `withContext` creates a new `Job` that does not have a corresponding coroutine.
- Jobs have a hierarchy in a tree structure.

## Cancellation propogration

- When a Job is cancelled, all its children will also be cancelled unless the Job is a `SupervisorJob`.
- CancellationException is not propogated outside the top level coroutine.
- Cancellatin signal traverses down the job hierarchy and cancellation exception traverses up the job hierarchy.

## NonCancellable vs Job

- Both works functionally as expected.

#### Recommendation

- Use `NonCancellable` to be clear of the intention.

# Parallel Decomposition

## Shared Mutable State

- Thread safe data structures
- Thread confinement (newSingleThreadContext)
- Mutual exclusion like mutex
  ```kotlin
  mutex.withLock {
    // code
  }
  ```

## Async Coroutine Builder

# Exceptions Handling

## Uncaught Exception in a Coroutine

## CoorutineExceptionHandler

## SupervisorJob

## Uncaught Exception in Async Coroutine
