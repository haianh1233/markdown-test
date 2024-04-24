# Table default

```sql records_count
SELECT
    count() AS count
FROM $topic
FORMAT JSON;
```

<Flex>
    <Statistic
        data={records_count}
        title='Total records count'
        value=count
    >
    </Statistic>
</Flex>