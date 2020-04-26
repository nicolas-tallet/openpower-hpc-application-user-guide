# Task Placement

## Default Placement

By default, task placement is performed according to the _group round-robin_ policy: each compute node gets as many MPI tasks as specified as number of tasks per node, one compute node after the other.

> The actual task placement can be checked through reading the rankfile of the job. The absolute path of this rankfile is provided by the `LSB_DJOB_RANKFILE` environment variable.

## User-Defined Placement

An alternate task placement can be requested by using the following environment variable: `LSB_TASK_GEOMETRY`. This environment variable allows the user to specify the group of MPI tasks that will coexist on the same compute node.

Example : Specifying an Alternate Task Placement through `LSB_TASK_GEOMETRY` Environment Variable

```text
export LSB_TASK_GEOMETRY="{(0,3)(1,4)(2,5)}"
```

The result of such a geometry would be the following:

* MPI Tasks \#0 and \#3 on node \#1
* MPI Tasks \#1 and \#4 on node \#2
* MPI Tasks \#2 and \#5 on node \#3

> The geometry does not define the identify for nodes \#1, \#2 and \#3. This assignment remains under the responsibility of the workload manager.

