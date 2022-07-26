# EPL Summary

## Pattern guards
[Pattern Guards](http://esper.espertech.com/release-5.4.0/esper-reference/html/event_patterns.html#pattern-guards)

### P1 - Took from the Documentation
```
every a=A -> (b=B where timer:within(2 sec))
```
```
every a=A -> b=B where timer:within (2 sec)
```
Match for each A event followed by a B event that arrives within 2 seconds after the A event.

### P2 - Took from the Documentation
```
every A  -> (B -> C) where timer:within(1 hour)
```
Match for each A event followed by a B event and followed by a C event.
Both the B event and the C event must arrive within 1 hour of the A event.

### P3
```
every ((A  -> B and not C) where timer:within(3 sec))
```


### P4
```
every  A  -> (B and not C where timer:within(3 sec))
```
Match for each A event followed by a B event.
In a time window of 3 seconds opened by A we can't have a "C" event.

ex. It's a match. C and B arrive at the same time, the below sequence order doesn't matter, 
what matters is the position with respect to the time specified with `t=t.plus(time)`
```
A={n=4}
t=t.plus(3 seconds)
C={n=1}
B={n=4}
```

ex. It matches. As we said the temporal window refers to B and 
- A1@0 and B1@2
- A2@3 and B2@8

```
A={n=1}
t=t.plus(2 seconds)
B={n=1}
t=t.plus(1 seconds)
A={n=2}
t=t.plus(4 seconds)
C={n=2}
t=t.plus(1 seconds)
B={n=2}
```
### P5
```
every  A  -> ((B and not C) where timer:within(3 sec))
```
Match for each A event followed by a B event in a time window of 3 seconds.
Before the B event we can't have a C event in a time window LESS (EQUAL is OK) of 3 seconds.

ex. It's not a match. Time window is too large 
```
A={n=4}
t=t.plus(3 seconds)
C={n=1}
B={n=4}
```

ex. It matches only A1@0 and B1@2.
```
A={n=1}
t=t.plus(2 seconds)
B={n=1}
t=t.plus(1 seconds)
A={n=2}
t=t.plus(4 seconds)
C={n=2}
t=t.plus(1 seconds)
B={n=2}
```
## Window operations

### Logical Sliding
```
from SmokeSensorEvent.win:time(time_period)
```

### Physical Sliding
```
from SmokeSensorEvent.win:length(size)
```

### Logical Tumbling
```
from SmokeSensorEvent.win:time_batch(time_period)
```

### Physical Tumbling
```
from SmokeSensorEvent.win:length_batch(size)
```

### Landmark
It's enught not to specify any window.
It keeps widening.

ex.
```
select sensor, avg(temperature)
from TemperatureSensorEvent
group by sensor;
```