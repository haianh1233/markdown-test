# This is H1
## This is H2
### This is H3

<DatePicker></DatePicker>

I think I'll use it to format all 
of my documents from now on.

```sql records_count
SELECT
    count() AS count
FROM $topic;
```

```sql table_size
SELECT
    formatReadableSize(sum(bytes)) as size
FROM system.parts
WHERE
    table = '$topic'
    AND database = '$cluster'
```


<Flex>
    <Statistic
        data={records_count}
        title='Total records count'
        value=count
    >
    </Statistic>
    <Statistic
        data={table_size}
        title='Table size'
        value=size
    >
    </Statistic>
</Flex>

I really 
like using Markdown.

```sql statistics
SELECT
    toStartOfMinute(event_time) AS event_time_m,
    count() AS count_batches,
    avg(query_duration_ms) AS avg_duration
FROM clusterAllReplicas(default, system.query_log)
WHERE (query_kind = 'Insert') AND (type != 'QueryStart') AND (event_time > (now() - toIntervalDay(2)))
GROUP BY event_time_m
ORDER BY event_time_m ASC
```

<Sparklines data={statistics}>
    <SparklinesLine title='Insert Count Batches' value=count_batches color='blue'>       
    </SparklinesLine>
    <SparklinesLine title='Insert Avg Duration' value=avg_duration color='black'>         
    </SparklinesLine>
</Sparklines>

<DataTable value={statistics}>
</DataTable>

```sql insert_batch_and_avg_time
    SELECT
    toStartOfInterval(event_time_m, INTERVAL 20 MINUTE) AS event_time_group,
    groupArray(count_batches) AS count_batches_array,
    groupArray(avg_duration) AS avg_duration_array
FROM (
    SELECT
        toStartOfMinute(event_time) AS event_time_m,
        count() AS count_batches,
        avg(query_duration_ms) AS avg_duration
    FROM clusterAllReplicas(default, system.query_log)
    WHERE (query_kind = 'Insert') AND (type != 'QueryStart') AND (event_time > (now() - toIntervalDay(2)))
    GROUP BY event_time_m
)
GROUP BY event_time_group
ORDER BY event_time_group ASC;
```

<DataTable title="Tables with Sparklines 1" value={insert_batch_and_avg_time}>
</DataTable>

<DatePicker></DatePicker>

<DataTable title="Tables with Sparklines 2" value={insert_batch_and_avg_time}>
 <SparkLines header='Batches Count' field='count_batches_array'></SparkLines>
</DataTable>

