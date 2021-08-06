# Flow in Kotlin
## Where Flow is used?
In the case I encountered today, it's related to fetching data from the network, updating data, and sharing data between different places. It helps in the case the UI can be automatically updated when the value updates.

## Basic takeaways
There are 3 main rolesin the flow: producer, consumer, and intermediary. The intermediary will transform a flow to another flow (think about what a map or filter would do).

### Producer 
- The producer will emit the data.
- No need to use `suspend` keyword. When it's called, the flow will be returned, but the code within it will not run yet.
- Could use suspend function within flow builder.

```
fun simple(): Flow<Int> = flow {
  for (i in 1..10) 
    emit(i)
}
```

### Customer
And the consumer will collect the data.
```
runBlocking {
  simple().collect { value ->
    println(value)
  }
}
```
Note that the code within the flow builder wouldn't run until collect is being called. 
And this example clearly shows this:
![image](https://user-images.githubusercontent.com/21307533/128456582-84c7d49e-52c4-4875-bc15-8d81db6da41c.png)


## Questions remaining
As shown in the video, the interfaces are pretty simple.
```
interface Flow<out T> {
  suspend fun collect(collector: FlowCollector<T>)
}

interface FlowCollector<in T> {
  suspend fun emit(value: T)
}

```
**But how does the interface work with the flow builder?** 

## References:
- https://kotlinlang.org/docs/flow.html
- [Youtube: Going with the flow - Kotlin Vocabulary](https://www.youtube.com/watch?v=emk9_tVVLcc)
- [KotlinConf 2019: Asynchronous Data Streams with Kotlin Flow by Roman Elizarov](https://www.youtube.com/watch?v=tYcqn48SMT8)
