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


<DataTable value={tables} searchValue="table">
</DataTable>
