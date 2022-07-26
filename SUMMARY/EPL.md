# EPL Summary

## Patterns

### Q1
```
every a=A -> (b=B where timer:within(2 sec))
```

### Q2 - Took from the Documentation
```
every A  -> (B -> C) where timer:within(1 hour)
```
Match for each A event followed by a B event and followed by a C event.
Both the B event and the C event must arrive within 1 hour of the A event.

### Q3
```
every ((A  -> B and not C) where timer:within(3 sec))
```


### Q4
```
every  A  -> (B and not C where timer:within(3 sec))
```
Match for each A event followed by a B event in a time window of 3 seconds.
In this time window, between these two events, we can't have a "C" event.
???

ex. It's a match.
```
A={n=4}
t=t.plus(3 seconds)
C={n=1}
B={n=4}
```

### Q5
```
every  A  -> ((B and not C) where timer:within(3 sec))
```
Match for each A event followed by a B event in a time window of 3 seconds.
Before the B event we can't have a C event in a time window LESS (EQUAL is OK) of 3 seconds.

ex. It's not a match.
```
A={n=4}
t=t.plus(3 seconds)
C={n=1}
B={n=4}
```