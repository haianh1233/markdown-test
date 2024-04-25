```sql tables
SELECT
    table,
    formatReadableSize(sum(bytes)) as size
FROM system.parts
WHERE active AND database != 'system'
GROUP BY table
FORMAT JSON;
```


<DataTable value={tables}>
    <Column field="table" header="Table name"></Column>
    <Column field="size" header="Size"></Column>
</DataTable>