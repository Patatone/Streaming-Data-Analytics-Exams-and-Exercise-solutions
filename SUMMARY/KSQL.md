# KSQL Summary

## Window operations

### Logical Sliding Window Example
** Can be implemented with a Hopping Window with ADVANCE BY 1 SECONDS**

### Physical Sliding Window Example
**Not supported**

### Logical Tumbling Window Example

```
SELECT SENSOR, AVG(temperature) AS AVG_TEMP
FROM Temperature_STREAM 
     WINDOW TUMBLING (SIZE 4 SECONDS)
GROUP BY SENSOR 
EMIT CHANGES;
```
### Physical Tumbling Window Example
**Not supported**

### Logical Hopping Window Example
```
SELECT SENSOR, AVG(temperature) AS AVG_TEMP
FROM Temperature_STREAM 
     WINDOW HOPPING (SIZE 4 SECONDS, ADVANCE BY 2 SECONDS)
GROUP BY SENSOR 
EMIT CHANGES;
```

## Join operations

### Stream-to-Stream Join
```
SELECT *
FROM Temperature_STREAM T
  JOIN Smoke_STREAM S WITHIN 2 MINUTES
  ON S.sensor = T.sensor
 WHERE 
 T.temperature > 50 and
 S.smoke  and
 S.ts < T.ts 
 EMIT CHANGES;
```