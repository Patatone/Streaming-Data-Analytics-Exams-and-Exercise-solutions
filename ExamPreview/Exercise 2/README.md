![ExerciseText_N2](ExerciseText_N2.png)
# EPL

```
create schema DoorOpening(doorId int);
create schema DoorClosing(doorId int);

@Name("Out")
select doorId, count(t_open)
from DoorOpening.win:time(2 minutes)
group by doorId
output last every 60 seconds;

// NOT SURE
```

# kSQL

```
SELECT Open.id, count(Open)
FROM DoorOpening Open WITHIN 2 MINUTES
	JOIN DoorClosing Close
	ON Open.id = Close.id
	WINDOW TUMBLING (SIZE 1 MINUTES)
EMIT CHANGES;

// NOT SURE
```
