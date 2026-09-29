# DSA Assignment 2 - Hashing

## Problem

A railway booking system generates the following booking IDs:

23, 43, 13, 33, 53, 63, 73

The task is to implement and compare three collision-resolution
techniques:

1. Linear Probing
2. Quadratic Probing
3. Double Hashing

## Hash Table

Hash table size = 8

Primary hash function:

h(k) = k % 8

For double hashing:

h1(k) = k % 8

h2(k) = 1 + 2(k % 4)

## Load Factor

Number of keys = 7

Hash table size = 8

Load Factor = 7/8 = 0.875 = 87.5%

## Search

The following IDs were searched:

53, 63, 73, 80

53, 63 and 73 are existing booking IDs.

80 is a non-existing booking ID.

## Files

- main.c - C source code
- input.txt - Input data
- output.txt - Program output
- trace_table.txt - Trace tables
- complexity_analysis.txt - Complexity analysis
- comparison.txt - Performance comparison

## Conclusion

The experiment demonstrates the effect of different collision
resolution techniques on hash table performance. Linear probing is
simple but can suffer from primary clustering. Quadratic probing
reduces primary clustering, while double hashing provides a more
distributed probe sequence using a second hash function.

The load factor of 87.5% resulted in a high level of table occupancy,
making collision resolution important for efficient searching and
insertion.