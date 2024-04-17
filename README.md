# Concurrent, Lock-free Red-Black Trees

**Claire C. Chen** *(ccz@cs.cmu.edu)*  
**Joseph McLaughlin** *(jmmclaug@andrew.cmu.edu)*

---

## Work Completed So Far

We've completed a working sequential implementation of red-black trees based on casework outlined in [1], and created a testing framework with which to compare our sequential and parallel versions. We reviewed the relevant papers by van Breugel for inspiration on fine-grained lock-based approaches [2] [3], prior work on lock-free RBT implementation with Compare-and-Swap (CAS) [4], as well as Natarajan et. al's work on wait-free RBT's, which demonstrated that a lock-free implementation is still blocking [5]. Finally, we started implementing our lock-free version.

## Progress on Goals & Deliverables

By the end of week 1, our progress was on track, as we successfully completed our sequential implementation and reviewed the literature. For week 2, we decided to replace our original goal of creating a fine-grained locked-based implementation with getting an early start on the lock-free implementation, since it is the most technically challenging component of the project. For week 3, we continued our work with the lock-free implementation and further refined our testing code, identifying and fixing small bugs in our sequential implementation.

## Adjustments to Goals & Deliverables

Depending on our progress with the lock-free implementation, testing, and experimentation, we realized that we might not have time to create a fine-grained lock-based RBT implementation. As such, we have the following plan for our continued progress:

| Time          | Claire                                          | Joe                                           |
|---------------|-------------------------------------------------|-----------------------------------------------|
| Week 4.0      | Implement helper functions for local areas (markers and labels) to support bulk_insert and bulk_delete   | Implement helper functions for local areas (markers and labels) to support bulk_insert and bulk_delete |
| Week 5.0      | Draft bulk_insert function                      | Draft bulk_delete function                   |
| Week 5.0      | Look over and revise bulk_insert function       | Look over and revise bulk_delete function    |
| Week 5.5      | Continue work from week 5.0. If time, work on “nice to have" tasks | Continue work from week 5.0. If time, work on “nice to have" tasks   |
| Week 6.0      | Perform experiments on PSC/Xeon Phi             | Perform experiments on GHC machines           |
| Week 6.5      | Finalize items for presentation and website     | Finalize items for presentation and website |

Nice to haves:
1. Create a parallel red-black tree implementation that allows for mixed tests (deletes and inserts in the same test case). This has the added complexity of dealing with the case where an insert and delete want to simultaneously update the same value in the tree, and some ordering needs to be imposed on the two operations. As far as we know, this has not been done before.
2. Fine-grained or ticket-based locking implementation of RBT, as well as a performance analysis of the lock-based vs. lock-free versions. We decided to have the locking version be an extra goal to better focus on the lock-free implementation.

## Plans for the Poster Session

As of now, we plan to focus our poster on speedup graphs showing our parallel performance and scalability. We plan on presenting:

- Speedup graphs showing the performance of our parallel implementation on 1, 2, 4, and 8 cores on the GHC machines compared to our sequential implementation and our parallel performance on 1 core with various input sizes.
- Speedup graphs showing the performance of our implementation with high core counts on the PSC machines compared to our sequential implementation and our parallel performance on 1 core with various input sizes.

We may also create a visualization of our red-black tree output, though we feel this is less important as the final output doesn't give any insight to the efficiency of our implementation.

## Outstanding Concerns

- In addition to GHC machines, where else should we perform our experiments? Should we do it on 72-core Xeon Phi (we were told this could be made available to us for the final project)? Or PSC?
- We found a prior implementation with a similar goal of creating lock-free bulk insert and delete RBT operations. Hence, we're not sure if our project has sufficient novelty by using OpenMP instead of p\_threads for lock-free RBT. If not, how can we make it novel?

Otherwise, we are mostly confident we will be able to continue with the updated deliverables described above; we just need to keep working.

## References

[1] [https://en.wikipedia.org/wiki/Red–black_tree](https://en.wikipedia.org/wiki/Red–black_tree)  
[2] van Breugel, Frank. "A concise yet accurate proof that a lock-based red-black tree is non-blocking." (*Available upon request*)  
[3] van Breugel, Frank. "Fine-Grained Lock-Based Red-Black Trees." (*Available upon request*)  
[4] Kim, Jay. "Lock-Free Red-Black Tree Using Compare-and-Swap." (*Available upon request*)  
[5] Natarajan, Sriram, et al. "Concurrent wait-free red-black trees." (*Available upon request*)
