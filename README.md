## Details, v1

An eventbuf is persisted to a directory.

## What the Appender writes (to log events)

There are size-rotated append-only <code>events-...</code> files.

Their filenames are strictly increasing.

<pre>
            
my-eventbuf-dir/
  #       (rotation time)  (counter to avoid insanely rare collisions)
  events-2011-12-31-23-59-59-123-0001.v1
  events-2011-12-31-23-59-59-123-0002.v1
  events-2011-12-31-23-59-59-124-0001.v1
  ...
</pre>

Assumption (for now): there will never be multiple appenders or multiple readers running at once. Enforcing this is your responsibility (for now).

<pre>
FILE:
  EVENTs

EVENT:
  uint32le(data.length)
  uint64le(utc_ms)
  data
  4-byte hash: (sha256 of the above (12 + data.length) bytes)[0:4]
</pre>
