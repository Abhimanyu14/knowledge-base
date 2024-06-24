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
- `ensureActive()`

## The Importance of Cancellation Exception

## NonCancellable
