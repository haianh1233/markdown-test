
```sql tables
SELECT
    table AS tableName,
    sum(rows) AS totalRows,
    formatReadableSize(sum(data_uncompressed_bytes)) AS bytesUncompressed,
    formatReadableSize(sum(bytes_on_disk)) AS bytesOnDisk,
    concat(
        round(
            if(sum(bytes_on_disk) > 0, (sum(data_uncompressed_bytes) * 100 / sum(bytes_on_disk)) - 100, 0), 
            0)
        , '%') AS compressionRatio,
    formatReadableSize(sum(primary_key_size)) AS primaryKeySize,
    max(modification_time) AS lastModified
FROM
    system.parts
WHERE
    database = '$database'
GROUP BY
    table
ORDER BY
    sum(bytes_on_disk) DESC    
```


<DataTable value={tables} searchValue="tableName">
</DataTable>

```sql avg_query_duration
SELECT
    toStartOfMinute(event_time) AS event_time_m,
    count() AS count_batches,
    avg(query_duration_ms) AS avg_duration
FROM clusterAllReplicas(default, system.query_log)
WHERE (query_kind = 'Select') AND (type != 'QueryStart')
  AND (event_time >= now() - INTERVAL 2 HOUR) 
  AND (event_time <= now())
GROUP BY event_time_m
ORDER BY event_time_m ASC ;
```


<Flex>
    <LineChart
        title='Avg Query duration'
        data={avg_query_duration}
        x=event_time_m
        y=avg_duration>
    </LineChart>
    <LineChart
        title='Count batch'
        data={avg_query_duration}
        x=event_time_m
        y=count_batches>
    </LineChart>
</Flex>

