# SpecTM Design

## Speculative Redo Log

### Target 1: Order between Log and Data

In SpecTM, order between log and data in WAL is achieved by leveraging cache eviction. More specifically, the following two requirements ensure this order:

* **a)** log is created before data
* **b)** data and log can't coexist in any cache level

`a` ensures the initial order while `b` ensures this order holds up. `a` is easily implemented by write log before transactional operation. `b` is trickier in the sense that processor can't track data in cache hierarchy.

To overcome this delimma in pratical sense, we first consider a direct-mapping cache which map the memory address of same postfix to the same cacheline. By making the data and log share the same address postfix we can certainly ensure principle `b`. One more problem occurs in this setting that the read operation on data address will lead to a total eviction of all data sharing the same postfix, which leads to a point where log has to be flushed anyway. To tackle this problem, we can introduce data read redirect which lead transactional read to the nearest data sharing the same postfix. From the perspective of hardware design, there is no need to add more functional circuit to implement this machenism. Only modification is to add TXN bit to cache data and rewire  some of the existing circuits.

After direct-mapping cache, we must include solution for multi-way-mapping cache for its wide usage in commercial processor. multi-way cache is different in that certain address may only be mapped to the same group other that cacheline with address sharing postfix. In the process of retrieving, multiple address in a group is tested against request address simultaneously. Considering we already iadd TXN bit to cache data, we can leverage it to regroup testing circuit and test only the postfix-area tag bit, making multi-way cache no different than direct-mapping cache in logical sense.

One more thing to cover is the forging of log address. 

### Target 2: Acknowledgement for Data Grounding


### Target 3: Method for Strong Commit

## Speculative Transaction Execution

### Target 1: Dependency Tracking for Weak Conflict

### Target 2: Detect Strong Conflict

### Target 3: Fast Rollback