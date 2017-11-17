# Perf

The Linux standard Perf tool makes it possible to perform performance profiling of a specific process.

Perf mainly relies on the following commands:

| Command	      | Option  | Purpose
|:-------------:|:-------:|---------
| perf annotate |         | Annotate source code with events
| perf list     |         | List supported hardware counters
| perf record   |         | Record events
|               | -e rNNN	| Include specified hardware counter
|               | -g      | Include Call Graph
| perf report   |         | Display event by process / routine / …
                | -g      | Include Call Graph

## Procedure

### Performance Data Collection

The performance data collection is triggered by the *perf record* command.

The command can apply to:
* A binary execution:
```
perf record <binary>
```
* A given process designated by its PID:
```
perf record --pid=<pid>
```

Data collection produces an output file named *perf.data*.

### Performance Reporting

The *perf report* command produces a by-routine performance report:
```
perf report --input perf.data > perf.report
```

The *perf annotate* command generates line-by-line annotations inside the routines:
```
perf annotate --input perf.data > perf.annotate
```

## FlameGraph

* Download FlameGraph:
```
$ git clone https://github.com/brendangregg/FlameGraph
```

* Perform `perf` Recording with Option: `-g`

* Generate FlameGraph
```
$ perf script -i perf.data | stackcollapse-perf.pl | flamegraph.pl > flame-graph.svg
```
