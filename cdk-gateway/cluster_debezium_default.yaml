## This Table is debezium based

```sql records_count
SELECT
    count() AS count
FROM '$topic';
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
