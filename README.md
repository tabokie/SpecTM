# SpecTM

SpecTM is a novel durable transactional memory design that pursues extreme performance by allowing radical speculation. More design details and benchmark are under way.

## System Overview

SpecTM achieves high performance by two design improvements:

* Speculative Logging

In previous HTM design, WAL poses a critical path for transaction commit. While in SpecTM design, log is buffered with data in cache hierarchy and slowly evicted as transactional data flush in, which avoid the processor halting for log / data write order.

By using speculative to modify logging, we admit the compromise to strong persistency. Though it's often seen that a weaker persistency that can recover to consistent state is enough for most application, we offer solution for strong persistency fallback.

* Speculative Transaction

In previous HTM design, OCC is implemented through cache directory to rollback transaction when a Read / Write conflict happens.

In SpecTM design, processor uses a lazier conflict control that allows conflicting transaction to continue while recording the underlying dependency. Fatal conflict is detected through directory AND local dependency. In this way, more potential interleaving is achieved between transactions.
