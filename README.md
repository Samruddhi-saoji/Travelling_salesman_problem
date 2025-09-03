# Travelling_salesman_problem
Solving TSP using various algorithms, and comparing the performances of all the algorithms

---

# Custom TSP class
A simple Python implementation of a `TSP` class for representing and solving the **Traveling Salesman Problem (TSP)**.  
It allows you to define a set of cities, calculate the cost (total distance) of a route, and visualize the route.

## Attributes

### `TSP.City`
- **x**: X-coordinate of the city.  
- **y**: Y-coordinate of the city.  
- **index**: Index of the city in the TSP city list.  
- **name**: Name or label of the city.  

### `TSP`
- **n**: Number of cities in the problem.  
- **cities**: List of `City` objects representing all cities.
  

## Methods

### `TSP.City`
- **__repr__()**: Returns the name of the city when printed.  

### `TSP`
- **distance_btw(c1, c2)**: Returns the Euclidean distance between two cities.  
- **cost(route)**: Computes the total distance of a given route (including the return to the starting city).  
- **display(route)**: Displays a scatter plot of the cities with lines connecting them in the given route.  


## Example Usage

```python
import numpy as np
import matplotlib.pyplot as plt
from random import random

# Import the TSP class (assuming tsp.py contains the implementation)
from tsp import TSP

# Number of cities
n = 15
cities = []
max_dist = 200

# Randomly generate city data
for i in range(n):
    tup = (max_dist * random(), max_dist * random(), i, i+1)  # (x, y, index, name)
    cities.append(tup)

# Create TSP instance
tsp = TSP(n, cities)

# Example route (visiting cities in order)
route = tsp.cities

# Calculate cost
print("Total route cost:", tsp.cost(route))

# Display route
tsp.display(route)
```

---

# Algorithms implemented
- Simulated annealing
- Genetic Algorithm
   - Naive algorithm
   - Advanced version (evolved gen 0)
- A star
- Ant colony optimisation

---

# Performance
Total distance of the best route found: Ant colony optimisation > simulated annealing > Genetic algorithm > A star

--- 

# Improved Genetic Algorithm in detail
## Key Concepts

### 1. The Concept of "Above Average Solution"
- Randomly select pairs of cities (≈100 pairs) and compute their distances.  
- Take the average of these distances → **average path cost per step**.  
- Multiply this by the number of paths (number of cities, since we return to the starting city) → **expected average cost of a full route**.  
- Any solution with cost **less than this average cost** is considered an **above average solution**.

---

### 2. An Advanced Generation 0
- In a standard GA, generation 0 (initial population) is random.  
- Here, we **evolve generation 0** by filtering multiple random populations.  
- Process:
  1. For the first few iterations, generate random populations (without crossover or mutation).  
  2. Evaluate all solutions.  
  3. Keep only the **above average solutions** from these random populations.  
  4. Collect these into the **advanced Generation 0**.  

➡️ This ensures the algorithm starts with **stronger-than-random candidates**, similar to "sending the brightest humans to populate a distant planet."

---

### 3. Mutating Only Below Average Solutions
- In standard GA:
  - Mutation can ruin good solutions (optimal children can get worse).  
  - A low mutation rate is preferred but reduces diversity.  
  - Mutation is still needed to explore new paths.  
- Proposed strategy:
  - For each child solution:
    - If its cost ≤ average cost → **keep it unchanged** (above average, valuable).  
    - If its cost > average cost → **mutate it** (nothing to lose, chance to improve).  
  - Add both above average and (mutated) below average solutions to the next generation.  

➡️ This preserves good solutions and only applies mutation to weaker ones.

## Advantages
- **Better Initialization**: Advanced generation 0 with only above-average solutions.  
- **Smarter Mutation**: Apply mutation **only** to below-average solutions.  
- **Stronger Selection Pressure**: Keeps the population biased toward fitter candidates.  
- **Maintains Diversity** without sacrificing good solutions.

