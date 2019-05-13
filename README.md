# Solving-the-Joint-Order-Batching-and-Picker-Routing-Problem-for-Large-Instances

## Abstract

In this work, we investigate the problem of order batching and picker routing in warehouse storage areas. These problems are known to be capital and labour
intensive, and often contribute to a sizable fraction of warehouse operating costs. Here, we consider the case of online grocery shopping where orders may consist of dozens of items.

We present the problem introduced in [1] and tackle the issue of solving the problem heuristically with proposed methods of solving that utilize batching and
routing heuristics. Instances with up to 50 orders were solved heuristically in large simulated warehouse instances consisting of 8 to 30 aisles, with 1 to 4 blocks. The proposed methods were shown to have relatively short computation times as compared to optimally solving the problem in [1]. In particular, we showed that a proposed method which utilizes an optimal solver for routing yielded poorer results than methods that utilize routing heuristics.

**Keywords**: integer programming, inventory management, order batching, order picking, picker routing

## Methods Employed in Solving

### ILP Solver

To obtain optimal solutions to the ILP formulation, we employed a branch-and-cut solver to solve the formulation written in MiniZinc, an open-source constraint modeling language.

### Method 1

In this method, we first compute the order batching via the time savings heuristic (TSH) [2]. Picker routing heuristics were used to obtain estimates of the partial route distances, for use in the TSH. Partial routes were computed by running each of the following heuristics: Nearest Neighbor, S-shape and Largest gap.

### Method 2

This method involves the use of a heuristic with optimal routing for the final assignment of orders. In other words, we use the routing estimates obtained during the routing algorithms in the TSH (to batch orders), but once all orders have been assigned (batched), we solve for each picker optimally to find its optimal route.

For our experiments, in the order batching stage of Method 2, we employ the TSH which uses the routing heuristics introduced in Method 1. Then once order batches are computed in Python 3, to solve for each picker’s route optimally (with their batched order), we employ the Concorde TSP Solver. In particular, once
orders have been batched to each picker, the JOBPRP is equivalent to the general TSP problem [7] as picker capacities have already been handled in the routing heuristics in the TSH.

### Method 3

Here, instead of using routing heuristics to compute the routing estimates in order batching and routing of pickers after final assignment of batched orders, we employ the Concorde TSP solver [9] to compute optimal routes.

## Results

We first define the following:

$`\text{quality of solution} = \frac{\text{total distance without batching} - \text{objective value}}{\text{total distance without batching}}`$

where the total distance without batching is computed with only the routing heuristic (which is the trivial batching objective value, and the best routing heuristic was used - Optimal solver), and the objective value is the heuristic solution of the JOBPRP obtained by the respective Methods 1, 2 & 3. 

## References

[1] Cristiano Arbex Valle, John E. Beasley, and Alexandre Salles da Cunha. Optimally solving the joint order batching and picker routing problem. *European Journal of Operational Research*, 262(3):817 – 834, 2017.

[2] Kees Jan Roodbergen and RenÉde Koster. Routing methods for warehouses with multiple cross aisles. *International Journal of Production Research*, 39(9):1865–1883, 2001.

[7] Wolsey L. A. Integer programming. 1998.

[9] David L. Applegate, Robert E. Bixby, Vasek Chvatal, and William J. Cook. *The Traveling Salesman Problem: A Computational Study (Princeton Series
in Applied Mathematics)*. Princeton University Press, Princeton, NJ, USA, 2007.