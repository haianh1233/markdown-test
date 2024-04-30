```sql table_info
SELECT
    sum(rows) AS totalRows,
    formatReadableSize(sum(bytes_on_disk)) AS bytesOnDisk,
    formatReadableSize(sum(bytes_on_disk) / sum(rows)) AS averageRowSize,
    concat(
        round(
            if(sum(bytes_on_disk) > 0, (sum(data_uncompressed_bytes) * 100 / sum(bytes_on_disk)) - 100, 0), 
            0)
        , '%') AS compressionRatio
FROM
    system.parts
WHERE
    database = '$cluster' and table  = '$topic'
```

```sql compression_type
SELECT
    distinct(_batch_compression_type) as compressionType,
    count(*) as total
FROM '$topic'
GROUP BY _batch_compression_type
```

```sql timestamp_type
SELECT
    distinct(_batch_timestamp_type) as timestampType,
    count(*) as total
FROM '$topic'
GROUP BY _batch_timestamp_type
```

```sql by_schema
SELECT
    distinct(_value_schema_id) as schemaId,
    count(*) as total
FROM '$topic'
GROUP BY _value_schema_id
```

```sql batch 
SELECT
    formatReadableSize(avg(_batch_size)) as avgBatchSize,
    formatReadableSize(max(_batch_size)) as maxBatchSize,
    formatReadableSize(min(_batch_size)) as minBatchSize,
    avg(_batch_records) as avgRecordsPerBatch,
    max(_batch_records) as maxRecordsPerBatch,
    min(_batch_records) as minRecordsPerBatch,
    formatReadableSize(avg(_batch_size) / avg(_batch_records)) as avgRecordSize,
    count(distinct(_value_schema_id)) as nbSchemas
FROM '$topic'
```

<Flex>
    <Statistic
            data={table_info}
            title='Total Rows'
            value=totalRows></Statistic>
    <Statistic
            data={batch}
            title='Average record size'
            value=avgRecordSize></Statistic>
    <Statistic
            data={table_info}
            title='Bytes On Disk'
            value=bytesOnDisk></Statistic>
</Flex>

<Flex>
    <Statistic
            data={table_info}
            title='Average Row size'
            value=averageRowSize></Statistic>
    <Statistic
            data={table_info}
            title='Compression Ratio'
            value=compressionRatio></Statistic>
</Flex>


<Flex>
    <Statistic
            data={batch}
            title='Avg batch size'
            value=avgBatchSize></Statistic>
    <Statistic
            data={batch}
            title='Max batch size'
            value=maxBatchSize></Statistic>
    <Statistic
            data={batch}
            title='Min batch size'
            value=minBatchSize></Statistic>
</Flex>

<Flex>
    <Statistic
            data={batch}
            title='Avg records per batch'
            value=avgRecordsPerBatch></Statistic>
    <Statistic
            data={batch}
            title='Max records per batch'
            value=maxRecordsPerBatch></Statistic>
    <Statistic
            data={batch}
            title='Min records per batch'
            value=minRecordsPerBatch></Statistic>
</Flex>


<Flex>
    <Statistic
            data={batch}
            title='Nb schemas'
            value=nbSchemas></Statistic>
</Flex>

<Flex>
    <DataTable value={compression_type}></DataTable>
    <DataTable value={timestamp_type}></DataTable>
    <DataTable value={by_schema}></DataTable>
</Flex>
