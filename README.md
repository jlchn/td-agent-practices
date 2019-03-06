
### Size of the emitted data exceeds buffer_chunk_limit

Logs

```
Size of the emitted data exceeds buffer_chunk_limit.

This may occur problems in the output plugins ``at this server.``

To avoid problems, set a smaller number to the buffer_chunk_limit

```

we set `buffer_chunk_limit` as 64MB for the output plugin(s3 forest in our case), but there files more than 100MB inside of the buffer path, the warning message tells us `to avoid problems, set a smaller number to the buffer_chunk_limit`, but we fixed the issue by incresing the  `buffer_chunk_limit` to 256MB.


### no patterns matched tag="var.log.pods.service_name_0.log"

This usually means you have some file watched(defined within the `<source></source>` tag) but forgot to give a proper `<match></match>`
Solution could be:

1. give a match for this log
2. make the mapping under the `<source></source>` tag more strict
3. use below if you want to ignore the tags with no matches

```
<match var.log.pods.*.log>
  type null
</match>
```

