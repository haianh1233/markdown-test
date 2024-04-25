```sql compression_type
SELECT
    distinct(_batch_compression_type) as type,
    count(*) as total
FROM `local-kafka`.purchases
GROUP BY _batch_compression_type
FORMAT JSON
```





```sql avg_batch_record_size
SELECT
    avg(_batch_records) as avg,
FROM `local-kafka`.purchases
FORMAT JSON
```

<Flex>

<DataTable value={compression_type}>
    <Column field="type" header="Compression"></Column>
    <Column field="total" header="Total"></Column>
</DataTable>

<Statistic
        data={avg_batch_record_size}
        title='Number'
        value=avg
    >
</Statistic>

</Flex>
