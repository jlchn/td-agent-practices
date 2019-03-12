
### Size of the emitted data exceeds buffer_chunk_limit

Logs

```
Size of the emitted data exceeds buffer_chunk_limit.

This may occur problems in the output plugins ``at this server.``

To avoid problems, set a smaller number to the buffer_chunk_limit in the forward output ``at the log forwarding server.``

```

we set `buffer_chunk_limit` as 64MB for the output plugin(s3 forest in our case), but there files more than 100MB inside of the buffer path, we fixed the issue by incresing the  `buffer_chunk_limit` to 256MB.


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

### got incomplete line before first line from ...

from the source codes of td-agent, we found this warning appear when parsing multiline logs, in our case, the gc logs.

the multiple line got broken because of the the log rotation of gc logs, actually the the broken logs were already emitted.

so it is not critical for us.


https://github.com/fluent/fluentd/blob/v0.12.42/lib/fluent/plugin/in_tail.rb#L309

https://groups.google.com/forum/#!topic/fluentd/vrMLwsBT4_I

https://docs.fluentd.org/v0.12/articles/parser_multiline



