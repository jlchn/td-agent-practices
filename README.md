
### Size of the emitted data exceeds buffer_chunk_limit

Logs

```
Size of the emitted data exceeds buffer_chunk_limit.

This may occur problems in the output plugins ``at this server.``

To avoid problems, set a smaller number to the buffer_chunk_limit

```

we set `buffer_chunk_limit` as 64MB for the output plugin(s3 forest in our case), but there files more than 100MB inside of the buffer path, the warning message tells us `to avoid problems, set a smaller number to the buffer_chunk_limit`, but fixed the issue by incresing the  `buffer_chunk_limit` as 256MB.


