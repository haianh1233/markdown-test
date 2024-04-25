<DatePicker></DatePicker>
format yyyy-mm-dd hh-mm-ss , ie: 2024-04-23 15:44:16


```sql tables_count
SELECT COUNT(*) as count
FROM (
         SELECT
             database,
             table
         FROM clusterAllReplicas(default, system.parts)
         WHERE active
         GROUP BY
             database,
             table
     ) 
```

```sql gbs_count
SELECT
    ROUND(sum(bytes) / 1073741824, 5) AS gbs_count
FROM clusterAllReplicas(default, system.parts)
WHERE active;
```

```sql last_query_count
SELECT
    SUM(count) AS total_count
FROM (
    SELECT
        client_name,
        count() AS count,
        query_kind,
        toStartOfMinute(event_time) AS event_time_m
    FROM system.query_log
    WHERE (type = 'QueryStart')
      AND (event_time >= parseDateTimeBestEffort('$start_date'))  
      AND (event_time <= parseDateTimeBestEffort('$end_date'))
    GROUP BY
        event_time_m,
        client_name,
        query_kind
) AS grouped_results ;   
```

# Global overview of your cluster

<Flex>
    <Statistic
        data={tables_count}
        title='Tables count'
        value=count
    >
    </Statistic>
    <Statistic
        data={gbs_count}
        title='GBs count'
        value=gbs_count
    >
    </Statistic>
    <Statistic
        data={last_query_count}
        title='Last queries count'
        value=total_count
    >
    </Statistic>
</Flex>

```sql avg_query_duration
SELECT
    toStartOfMinute(event_time) AS event_time_m,
    count() AS count_batches,
    avg(query_duration_ms) AS avg_duration
FROM clusterAllReplicas(default, system.query_log)
WHERE (query_kind = 'Select') AND (type != 'QueryStart')       
  AND (event_time >= parseDateTimeBestEffort('$start_date'))
  AND (event_time <= parseDateTimeBestEffort('$end_date'))
GROUP BY event_time_m
ORDER BY event_time_m ASC ;
```


<Flex>
    <LineChart
        title='Avg Query duration from $start_date to $end_date'
        data={avg_query_duration}
        x=event_time_m
        y=avg_duration>
    </LineChart>
    <LineChart
        title='Count batch from $start_date to $end_date'
        data={avg_query_duration}
        x=event_time_m
        y=count_batches>
    </LineChart>

</Flex>




```sql query_logs
SELECT * FROM system.query_log
WHERE (event_time >= parseDateTimeBestEffort('$start_date'))
  AND (event_time <= parseDateTimeBestEffort('$end_date'))
order by event_time desc;
```
# Query logs table from $start_date to $end_date

<DataTable value={query_logs}>
    <Column field="hostname" header="Host name"></Column>
    <Column field="type" header="Type"></Column>
    <Column field="event_date" header="Event Date"></Column>
    <Column field="event_time" header="Event Time"></Column>
    <Column field="read_rows" header="Read Rows"></Column>
</DataTable>





