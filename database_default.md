```sql tables
SELECT
    table,
    formatReadableSize(sum(bytes)) as size,
    formatReadableSize(sum(primary_key_size)) as primary_key_size
FROM system.parts
WHERE active AND database != 'system'
GROUP BY table
```


<DataTable value={tables}>
    <Column field="table" header="Table name"></Column>
    <Column field="size" header="Size"></Column>
</DataTable>