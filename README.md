# Update PostgreSQL settings in Docker container

In this playground, I want to verify that I can change the PostgreSQL configuration settings even after first initialization.

```sh
$ docker compose up -d --wait
```

Default PostgreSQL configuration parameters:

```sh
$ ./scripts/show-pg-config.sh | grep "^ \(work_mem\|seq_page_cost\|random_page_cost\|effective_io_concurrency\|checkpoint_completion_target\|jit_optimize_above_cost\|jit_inline_above_cost\|jit_above_cost\)"

 checkpoint_completion_target           | 0.9                                        | Time spent flushing dirty buffers during checkpoint, as fraction of checkpoint interval.
 effective_io_concurrency               | 1                                          | Number of simultaneous requests that can be handled efficiently by the disk subsystem.
 jit_above_cost                         | 100000                                     | Perform JIT compilation if query is more expensive.
 jit_inline_above_cost                  | 500000                                     | Perform JIT inlining if query is more expensive.
 jit_optimize_above_cost                | 500000                                     | Optimize JIT-compiled functions if query is more expensive.
 random_page_cost                       | 4                                          | Sets the planner's estimate of the cost of a nonsequentially fetched disk page.
 seq_page_cost                          | 1                                          | Sets the planner's estimate of the cost of a sequentially fetched disk page.
 work_mem                               | 4MB                                        | Sets the maximum memory to be used for query workspaces.
```

Next, uncomment this line in `docker-compose.yml` to update PostgreSQL configuration parameters:

```
    # command: >
    #   -c work_mem=50MB
    #   -c seq_page_cost=0.8
    #   -c random_page_cost=1.1
    #   -c effective_io_concurrency=200
    #   -c checkpoint_completion_target=0.9
    #   -c jit_optimize_above_cost=3000000
    #   -c jit_inline_above_cost=1000000
    #   -c jit_above_cost=3000000
```

```sh
$ docker compose up -d --wait
```

Check PostgreSQL settings have been updated with success:

```sh
$ ./scripts/show-pg-config.sh | grep "^ \(work_mem\|seq_page_cost\|random_page_cost\|effective_io_concurrency\|checkpoint_completion_target\|jit_optimize_above_cost\|jit_inline_above_cost\|jit_above_cost\)"
 checkpoint_completion_target           | 0.9                                        | Time spent flushing dirty buffers during checkpoint, as fraction of checkpoint interval.
 effective_io_concurrency               | 200                                        | Number of simultaneous requests that can be handled efficiently by the disk subsystem.
 jit_above_cost                         | 3e+06                                      | Perform JIT compilation if query is more expensive.
 jit_inline_above_cost                  | 1e+06                                      | Perform JIT inlining if query is more expensive.
 jit_optimize_above_cost                | 3e+06                                      | Optimize JIT-compiled functions if query is more expensive.
 random_page_cost                       | 1.1                                        | Sets the planner's estimate of the cost of a nonsequentially fetched disk page.
 seq_page_cost                          | 0.8                                        | Sets the planner's estimate of the cost of a sequentially fetched disk page.
 work_mem                               | 50MB                                       | Sets the maximum memory to be used for query workspaces.
```

Ressources:

- https://postgresqlco.nf/
- https://wiki.postgresql.org/wiki/Tuning_Your_PostgreSQL_Server
