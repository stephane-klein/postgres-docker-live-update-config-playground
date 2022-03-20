#

```sh
$ docker compose up -d --wait
```

Default PostgreSQL configuration parameters:

```
$ ./scripts/show-pg-config.sh | grep "^ \(work_mem\|seq_page_cost\|random_page_cost\|effective_io_concurrency\|checkpoint_completion_target\|jit_optimize_above_cost\|jit_inline_above_cost\|jit_above_cost\)"

 random_page_cost                       | 4                                          | Sets the planner's estimate of the cost of a nonsequentially fetched disk page.
 seq_page_cost                          | 1                                          | Sets the planner's estimate of the cost of a sequentially fetched disk page.
 work_mem                               | 4MB                                        | Sets the maximum memory to be used for query workspaces.
```

Update this PostgreSQL configuration parameters:

```
work_mem=50MB
seq_page_cost=0.8
random_page_cost=1.1
effective_io_concurrency=200
checkpoint_completion_target=0.9
jit_optimize_above_cost=3000000
jit_inline_above_cost=1000000
jit_above_cost=3000000
```


Ressources:

- https://postgresqlco.nf/
- https://wiki.postgresql.org/wiki/Tuning_Your_PostgreSQL_Server
