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

###

###

###
