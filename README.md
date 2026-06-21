# CBUS Branch and Bound

## 1. Main idea
The CBUS problem requires finding the shortest route for a bus starting at point 0, serving $n$ passengers (picking up at point $i$ and dropping off at point $i + n$) and return back to point 0, with maximum capacity of the bus is $k$.

The Branch and Bound algorithm solves this problem by traversing possible routes, but using a pruning mechanism to remove routes that can not contain a optimal solution.

* **Branching:** Construct the route step by step, at every step, try selecting the next valid point (pick up or drop off).
* **Bounding:** At each step, calculate the lower bound for the cost of the routes extended from the current state.
* **Pruning:** Save the incumbent which is the cost of the best completed route found so far. If the total cost of the traveled route and the lower bound of the remaining path is greater than or equal the incumbent, stop expanding this branch due to finding a better solution is not possible.

---

## 2. Modelling

### 2.1. Variables
* `$x[i]$`: Variable representing the $i$-th stop of the route.
* `$visited[v]$`: Variable noting whether the point $v$ has been visited or not.
* `$load$`: Variable tracking the number of passengers currently on the bus.

### 2.2. Domains
* $D(x[i]) = \{1, 2, ..., 2n\}$: The bus can only choose valid pick up or drop off points.
* $D(visited[v]) = \{0, 1\}$: The state can only be visited (1) or not visited (0).
* $D(load) = \{0, 1, ..., k\}$: The bus load can not be negative and is upper bounded by the maximum capacity $k$.

### 2.3. Constraints
* **Unvisited constraint:** Requires all $x[i]$ must be distinct to ensure that the bus won't visit a point twice.
* **Pick up/Drop off constraint:** With every drop off point $v > n$, the pick up point $v - n$ must appear before in the route.
* **Capacity constraint:** At any given time, the value of $load$ must stay within its domain.

---

## 3. Objective function
The goal is to minimize total travel distance of the bus visiting all $2n$ points and returning to 0.

$$f(x) = \sum_{i=0}^{2n-1} c(x[i], x[i+1]) + c(x[2n], 0) \rightarrow \min$$

**Where:**
* $c(i, j)$ is the distance matrix from point $i$ to point $j$.
* $x[0] = 0$ is the starting point.
* $x[1 \dots 2n]$ is the permutation of $2n$ valid pick up and drop off points.
