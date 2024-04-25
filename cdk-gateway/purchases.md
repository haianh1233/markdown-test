```sql compression_type
SELECT
    distinct(_batch_compression_type) as type,
    count(*) as total
FROM `local-kafka`.purchases
group by _batch_compression_type
```


<DataTable value={compression_type}>
    <Column field="type" header="Compression"></Column>
    <Column field="total" header="Total"></Column>
</DataTable>
