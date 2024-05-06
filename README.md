# Concurrent, Lock-free Red-Black Trees

**Claire C. Chen**  
School of Computer Science  
Carnegie Mellon University  
Pittsburgh, PA  
[ccz@cs.cmu.edu](mailto:ccz@cs.cmu.edu)  

**Joseph McLaughlin**  
School of Computer Science  
Carnegie Mellon University  
Pittsburgh, PA  
[jmmclaug@andrew.cmu.edu](mailto:jmmclaug@andrew.cmu.edu)  

---

**CMU 15-418 Parallel Computer Architecture and Programming**  
*Final Project Report*  
*May 5, 2024*

## Abstract

We present an OpenMP implementation of parallel, lock-free Red-black Trees supporting concurrent insertion operations under the shared-address space model based on previously designed algorithms. We iterated upon a prior implementation and pseudocode described in the literature, identifying sources of correctness issues. Our implementation achieved a sublinear speedup on large input test cases.

## Notation

Throughout this report we refer to nodes by their relation to the critical nodes in various Red-Black operations such as the nodes being inserted. We use the following naming convention based on the tree below:

![Notation Tree](background/Notation Tree.png)

| Short Name | Full Name |
|------------|-----------|
| x          | Node      |
| p          | Parent    |
| c          | (Either) Child |
| s          | Sibling   |
| dn         | Distant Nephew |
| cn         | Close Nephew   |
| gp         | Grandparent    |
| p          | Uncle      |

We additionally use the following terms not shown in the tree:
- An *Outer Child* is a node whose direction from its parent is the same as the parent's direction from the grandparent.
- An *Inner Child* is a node whose direction from its parent is opposite of the parent's direction from the grandparent.

## Background

### Data Structure

Binary Search Trees are useful in a variety of information storage and retrieval algorithms for having potentially sublinear insertion and deletion costs but can't guarantee asymptotically better performance on their own due to imbalancing. Various balancing schemes exist to achieve faster guaranteed bounds on insertion and deletion time at the cost of algorithm intricacy. Red-Black Trees are one commonly used self-balancing binary search tree that achieves logarithmic bounds for common operations such as insertion and deletion.

### Opportunities for Parallelism

Opportunities for parallelism come from exploiting a red-black Tree's spatial locality in self-balancing operations when inserting or deleting a node. In particular, an insertion operation requires at most two rotations, while deletion only uses at most three. Additionally, both algorithms only recolor nodes on the general path between the starting leaf and the root node as per the above flowchart, where all reads and writes occur with nodes a constant distance away from the critical node. This leads to the opportunity of two insertion operations occurring simultaneously without interference so long as they are guaranteed to work on distant enough portions of the tree. This provides room for parallelism in bulk insertion operations, particularly for inputs with random values.

### Approach

#### Theory

The complexity in parallelizing red-black trees occurs with two threads simultaneously working in nearby areas, as the changes caused by one thread can affect the correctness of the other's operations. In particular, insertion operates on four nearby nodes as shown below. To implement this, Ma's lock-free insertion algorithm suggests we mark each operation's area of effect, or *local area*, by setting the `flag` field in each node in the local area. By performing a compare-and-swap operation for acquisition of a node's flag, we can ensure no two operations are applied to the same node at the same time.

#### Implementation

We used OpenMP for our implementation since it is the most popular and widely used shared-memory parallel programming API. Since OpenMP is available in C, C++, and Fortran, we selected C++ for its support for convenient pointer manipulation and automatic type inference, as Red-Black Trees are a complex, multi-link data structure. We first created a sequential implementation of Red-Black trees adapted from the Wikipedia page on Red-Black Trees.

#### Results

To benchmark our implementation, we ran our code on the 8-core GHC cluster machines with randomized inputs of various sizes. We created a testing script (`test-gen.py`) to allow us to generate test cases with scenarios simulating any number of concurrent RBT operations at each timestep, as well as the number of timesteps. We utilized trace-based simulation in order to mirror how red-black trees are used to store, query, and modify data, as real-world systems are designed to handle concurrent requests.

## Results

To benchmark our implementation, we ran our code on the 8-core GHC cluster machines with randomized inputs of various sizes. We created a testing script (`test-gen.py`) to allow us to generate test cases with scenarios simulating any number of concurrent RBT operations at each timestep, as well as the number of timesteps. We utilized trace-based simulation in order to mirror how red-black trees are used to store, query, and modify data, as real-world systems are designed to handle concurrent requests.

## Work Distribution

Overall, the work distribution was **50\% - 50\%**, with both team members contributing an equal amount. The two team members completed the sequential and parallel implementation synchronously either in-person or via Zoom and VSCode Liveshare. In particular, Joe led the effort with the sequential implementation, while Claire led the effort on our testing schema. Our utility functions and parallel implementation were done synchronously and thus reflect an equal effort from both of us.
