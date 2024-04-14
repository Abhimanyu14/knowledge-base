### To create a new coroutine context

```
private val coroutineScope = CoroutineScope(Dispatchers.Main.immediate)
```

### To launch a coroutine

```
coroutineScope.launch {
    // coroutine code
}
```

### Coroutine launcher job & cancellation

```
private var job: Job? = null

override fun lifecycleMethod() {
    job = coroutineScope.launch {
        // coroutine code
    }
}

override fun onStop() {
    job?.cancel()
}

```

### To switch context

```
withContext(context: CoroutineContext) {
    // coroutine code
}
```

### To return data from withContext

```
withContext(context: CoroutineContext) {
    // coroutine code
    returnValue // The last line will be evaluated and returned
}
```

### Delay

Use `delay()` in coroutines instead of `Thread.sleep()`.

- `delay()` - Suspension
- `Thread.sleep()` - Blocking

### Concurrency

Use multiple coroutine builders to start concurrent coroutines.

### Suspension vs Blocking

- Threads are blocking
- Coroutines are suspending

### Cancellation using Coroutine scope

```
coroutineScope.coroutineContext.cancelChildren()
```

### Coroutine cancellation vs coroutine scope children cancellation

- Coroutine cancellation - New coroutines can not be launched in a cancelled scope.
- There will be no error or exception.

```
coroutineScope.coroutineContext.cancelChildren()
vs
coroutineScope.cancel()
```

### To create a coroutine independent of the screen

[Coroutines & Patterns for work that shouldn’t be cancelled](https://medium.com/androiddevelopers/coroutines-patterns-for-work-that-shouldnt-be-cancelled-e26c40f142ad)

### Coroutines Best practices

[Best practices for coroutines in Android](https://developer.android.com/kotlin/coroutines/coroutines-best-practices#coroutine-cancellable)

[The Silent Killer That’s Crashing Your Coroutines](https://betterprogramming.pub/the-silent-killer-thats-crashing-your-coroutines-9171d1e8f79b)
[Coroutines codegen](https://github.com/JetBrains/kotlin/blob/master/compiler/backend/src/org/jetbrains/kotlin/codegen/coroutines/coroutines-codegen.md)
