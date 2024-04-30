

<Flex>

<DataTable value={compression_type}>
    <Column field="type" header="Compression"></Column>
    <Column field="total" header="Total"></Column>
</DataTab>

<Statistic
        data={avg_batch_record_size}
        title='Number'
        value="avg"
    >
</Statistic>

</Flex>
