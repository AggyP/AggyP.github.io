---
title: Using Genetic Algorithm to solve firestation problem 
date: 2021-10-28
categories: [Assignment]
math: true
---

**Programming Tool** - Python

**Technique** - Genetic Algorithm


## Firestation Problem Modeling

District X has 42 small towns (towns 1 to 42). The district council is in the process of determining the locations of building fire stations to serve the community of these 42 towns.

The district council has limited fund and the council wants to build the minimum number of fire stations. Each town is a potential place where a fire station can be located.


### List of variables




The goal of this assignment is to determine the minimum fire stations to be built in order to serve all the towns such that the driving time between town is less or equal than the maximum driving times. The objective (1) is to minimize the number of fire station built. Constraint (2) and (3) are to ensure at least one town built with fire station is within a specific driving times of each town.


### Problem formulation



### Representation

A binary representation is used to represent the fire station set covering problem where the length of the chromosome equals the total number of towns in the distinct which is 42. 1 in the first gene means the fire station will built in town 1. Figure 1.1 shows an example of a chromosome solution in which the total number of towns is 10 and the fire station will be built in town 1, 5, 8 and 10.


## Methodology