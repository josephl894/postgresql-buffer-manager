# PostgreSQL Buffer Manager

A replacement for PostgreSQL's default buffer replacement rule. The new rule is first-in, first-out: when memory is full, it discards the oldest unused page.

At 1MB of memory the simpler rule matched the speed of PostgreSQL's default. The comparison used the SQL cases in `project_1_test_cases/`.

## Code

PostgreSQL keeps this logic in `src/backend/storage/buffer/freelist.c`. Two versions are included:

- `freelist.c` walks the buffers in order and returns the oldest one that is not in use
- `another_way.c` uses PostgreSQL's small ring of buffers as the queue. That ring has a fixed length, so it is not the version that matched the default

`Dockerfile` is the Ubuntu environment used to build and run PostgreSQL. It does not itself apply the patch. Copy `freelist.c` over `src/backend/storage/buffer/freelist.c` in a PostgreSQL 16 tree, then compile.
