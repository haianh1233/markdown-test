```sql avg_batch_record_size
SELECT
    avg(_batch_records) as avg,
FROM purchases
```

```sql compression_type
SELECT
    distinct(_batch_compression_type) as type,
    count(*) as total
FROM purchases
GROUP BY _batch_compression_type
```

<Flex>

<DataTable value={compression_type}>
</DataTab>

<Statistic
        data={avg_batch_record_size}
        title='Number'
        value="avg"
    >
</Statistic>

</Flex>
