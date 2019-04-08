### td agent is tailing too manay files which reaches he upper limit of the file open number
the app services was outputing too many logs that the td agent cannot tail anymore.

we then add the `limit_recently_modified 3d` to the configuration to tail the recent 3 days logs.


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

https://github.com/fluent/fluentd/pull/1416

### the max size the td-agent will take in the buffer folder

max_buffer_size = chunk_size * queue_length + chunk_size * log_type_size

### debug 
```
<source>
 @type tail
 path /var/log/test.log.*.current,/var/log/test.log
 pos_file /var/log/td-agent/pos/test.pos
 format none
 time_format %Y-%m-%dT%H:%M:%S.%L%Z
 time_key time
 tag debug.test
</source>

<match debug.test>
 @type stdout
</match>
```
