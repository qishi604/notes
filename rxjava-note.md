# Rxjava for android 

### Observables

Observables are exactly like Iterables in this respect: they provide objects access to a stream of data with a well-defined termination point

The key difference between Observables and Iterators is that Observables provide access to asynchronous data streams while Iterables provide access to synchronous ones.


```java
for (Story story : stories) { 
	Log.i(TAG, story.getTitle());}

for (Iterator<Story> iterator = stories.iterator(); itera tor.hasNext();) {        Story story = iterator.next();        Log.i(TAG, story.getTitle());    }
```

subscribOn 决定产生事件的线程，
observeOn 决定消费事件的线程