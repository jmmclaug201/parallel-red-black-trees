# Parallel Red-Black Trees Project Proposal

**Authors:** Claire Chen, Joseph McLaughlin

**URL:** [https://github.com/jmmclaug201/parallel-red-black-trees/wiki](https://github.com/jmmclaug201/parallel-red-black-trees/wiki)

## Summary

In this project, we aim to create parallel implementations of Red-Black Trees to support element insertion, lookup, and deletion operations. Our objective is to implement both lock-based and lock-free versions of Red-Black Trees and evaluate their performance on the GHC Cluster machines.

## Background

Red-Black Trees are a type of self-balancing binary search tree known for their guaranteed O(log n) asymptotic height, ensuring efficient insertion, lookup, and deletion operations. They provide constant-time amortized cost for insertion and deletion. Red-Black Trees maintain several invariants beyond the traditional BST ordering:
- All nodes are labeled as either "Red" or "Black," with all NULL leaves considered black.
- A red node cannot have a red node as its parent.
- All paths from a node to its descendant leaves must have the same number of black nodes.

These constraints enable efficient insertion and deletion while preserving the Red-Black tree invariants through rotations and color adjustments.

Red-Black Trees are valuable in sequential settings for implementing data structures like sets. However, parallelism can benefit operations on large trees, especially bulk insertion and deletion tasks. Red-Black Trees are particularly suited for parallel bulk insertion due to requiring fewer rotations on average than other self-balancing trees like AVL trees.

## Challenges

Insertion into a regular binary tree can be easily parallelized since each insertion involves adding a node as a child of another node, allowing concurrent insertions without affecting each other. However, Red-Black Trees require local rebalancing operations during insertion and deletion, necessitating coordination among threads to maintain the tree's shape. Lock-based approaches may lead to inefficiencies due to potential contention, while designing a lock-free approach is challenging due to dynamic changes in the tree structure.

## Resources

We will structure our project based on pseudocode and algorithm descriptions from the following papers:
- Lock-based:
  - [PDF](https://wiki.eecs.yorku.ca/course_archive/2015-16/F/6490A/_media/public:myassignment1.pdf)
  - [Springer](https://link.springer.com/chapter/10.1007/978-3-319-03089-0_4)
- Lock-free:
  - [PDF](https://home.cs.umanitoba.ca/~hacamero/Research/RBTreesKim.pdf)
We will also explore additional literature on Parallel Red-Black Trees.

## Goals and Deliverables

**Need to Achieve:**
- Create lock-free implementations of Red-Black Trees supporting concurrent bulk insertion and deletion while maintaining invariants.

**Want to Achieve:**
- Develop scalable lock-free implementations capable of efficient bulk operations while preserving tree invariants.

**Hope to Achieve:**
- Optimize implementations for scaling to high-core count architectures with non-decreasing speedup up to 72 cores (Intel Xeon Phi) or 128 cores (PSC machines).

## Platform Choice

We will implement Red-Black Trees in C++ using OpenMP. Unlike MPI, OpenMP under the shared address model incurs less communication and synchronization overhead, aligning with our goals of creating and parallelizing lock-based and lock-free implementations efficiently.

## Schedule

### Week 1 (3/27 - 3/31)
- Implement a working sequential version of Red-Black Trees.
- Continue studying papers on parallelism for Red-Black Trees.
- Develop a high-level algorithm for lock-free implementation.

### Week 2 (4/1 - 4/7)
- Identify critical sections of the sequential algorithm.
- Implement a naive lock-based version supporting insertion, deletion, and lookup.
- Conduct correctness testing on the lock-based implementation.

### Week 3 (4/8 - 4/14) and Week 4 (4/15 - 4/21)
- Initiate work on the lock-free implementation.

### Week 5 (4/22 - 4/29)
- Identify optimization opportunities for both lock-free and lock-based implementations, especially for higher core counts.
- Implement optimizations.

### Week 6 (4/30 - 5/5)
- Perform experiments, fine-tune hyperparameters, and report findings.

This schedule provides a structured approach to achieving our project goals within the allocated time frame.
