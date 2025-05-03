# Speed-up compiler

## Concurrency with Eio

Maybe?

## Logging and formatting

Currently the logger is based on `Printf` module, which is blocking call.

Logging shoud be concurrent, formatting should be lazy.

```
(during priority task)
Log -> log_queue + timestamp

await priority
log_queue->drain
```

And make the output compatible with the OTLP format (the OpenTelemetry)

So we can analysis compiler process with tools like ZipKin or Jaeger.

## Retain type information

ReScript calls too many file IO to read/parse `*.cmt` files.

## OCaml native program

Make it (mostly) OCaml native program.

Also build system, package manager, etc. Then itegrate better.

## Memory-mapped database for metadata

Probably LMDB?

Pre-compiled libraries contains
- Metadata DB
- JavaScript artifacts

But sources still necessary... for sourcemap, debugging, etc
