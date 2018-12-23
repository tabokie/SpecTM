# SpecTM Design

## Speculative Redo Log

### Target 1: Order between Log and Data

In SpecTM, order between log and data in WAL is achieved by leveraging cache eviction. More specifically, the following two requirements ensure this order:

* **a)** log is created before data
* **b)** data and log can't coexist in any cache level

`a` ensures the initial order while `b` ensures this order holds up. `a` is easily implemented by write log before transactional operation. `b` is trickier in the sense that processor can't track data in cache hierarchy.

### Target 2: Acknowledgement for Data Grounding

### Target 3: Method for Strong Commit

## Speculative Transaction Execution

### Target 1: Dependency Tracking for Weak Conflict

### Target 2: Detect Strong Conflict

### Target 3: Fast Rollback