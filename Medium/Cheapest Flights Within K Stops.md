### Code
```cpp
class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        vector<int> dist(n, INT_MAX);
        dist[src]=0;
        
        // Step 2: Bellman-Ford Algorithm, modified to run k+1 times
        for (int i=0; i<=k; ++i) {
            vector<int> temp=dist; // Temporary copy to use the previous state
            
            // Step 3: Relaxation of all edges
            for (const auto& flight : flights) {
                int u=flight[0];
                int v=flight[1];
                int w=flight[2];
                
                if (dist[u]!=INT_MAX && dist[u]+w<temp[v]) {
                    temp[v]=dist[u]+w;
                }
            }
            dist=temp;
        }
        return dist[dst]==INT_MAX?-1:dist[dst];
    }
};
```

### Explanation

1. **Initialization**:
   - We initialize a vector `dist` of size `n` (the number of cities) with `INT_MAX` to represent the initial state where all cities are unreachable from the source except the source city itself. We set `dist[src] = 0` because the cost to reach the source city from itself is zero.

2. **Modified Bellman-Ford Algorithm**:
   - We run a loop for `k+1` iterations. Each iteration allows us to consider one more edge in the path, so `k+1` iterations correspond to considering up to `k` stops (which means using up to `k+1` edges).
   
3. **Relaxation of Edges**:
   - During each iteration, we use a temporary vector `temp` to hold the updated shortest distances based on the current iteration's edge relaxations. This helps us avoid premature updates that could affect the outcome of the same iteration.
   - For each flight `[u, v, w]`:
     - If `dist[u]` is not `INT_MAX` (meaning city `u` is reachable), and the new calculated distance to `v` (`dist[u] + w`) is less than `temp[v]`, we update `temp[v]` to `dist[u] + w`.

4. **Updating Distances**:
   - After checking all the flights, we update `dist` with values from `temp`. This ensures that after each iteration, we only consider the shortest paths with up to the current number of stops.

5. **Return Result**:
   - After `k+1` iterations, if `dist[dst]` is still `INT_MAX`, it means there is no valid route to the destination with at most `k` stops, so we return `-1`. Otherwise, we return the value of `dist[dst]`, which is the minimum cost to reach the destination.

### Time Complexity Analysis

- **Time Complexity**: The algorithm runs for `k+1` iterations, and in each iteration, it processes all the `flights`. Let `E` be the number of flights. The time complexity is thus `O((k+1) * E)`, which simplifies to `O(k * E)` because we only care about the order of growth.

### Space Complexity Analysis

- **Space Complexity**: We use two vectors of size `n` (the `dist` vector and the temporary `temp` vector). Hence, the space complexity is `O(n)`.

### Dry Run

Let's perform a dry run of the algorithm on a sample input to illustrate how it works.

**Sample Input**:

- `n = 4` (number of cities: 0, 1, 2, 3)
- `flights = [[0, 1, 100], [1, 2, 100], [0, 2, 500], [2, 3, 100]]` (flight information)
- `src = 0` (source city)
- `dst = 3` (destination city)
- `k = 1` (maximum number of stops allowed)

**Initial Setup**:

- `dist = [0, INT_MAX, INT_MAX, INT_MAX]` (distance to source city is 0, others are unreachable)
  
#### Iteration 1 (`i = 0`):

- `temp = [0, INT_MAX, INT_MAX, INT_MAX]` (copy of `dist` at the start of this iteration)

1. **Flight [0, 1, 100]**:
   - `dist[0] + 100 < temp[1]` ⟹ `0 + 100 < INT_MAX`, so `temp[1] = 100`.
   - `temp = [0, 100, INT_MAX, INT_MAX]`.

2. **Flight [1, 2, 100]**:
   - `dist[1]` is `INT_MAX`, so no update.

3. **Flight [0, 2, 500]**:
   - `dist[0] + 500 < temp[2]` ⟹ `0 + 500 < INT_MAX`, so `temp[2] = 500`.
   - `temp = [0, 100, 500, INT_MAX]`.

4. **Flight [2, 3, 100]**:
   - `dist[2]` is `INT_MAX`, so no update.

- **Update `dist` with `temp`**: `dist = [0, 100, 500, INT_MAX]`.

#### Iteration 2 (`i = 1`):

- `temp = [0, 100, 500, INT_MAX]` (copy of `dist` at the start of this iteration)

1. **Flight [0, 1, 100]**:
   - `dist[0] + 100 < temp[1]` ⟹ `0 + 100 < 100`, no update needed.

2. **Flight [1, 2, 100]**:
   - `dist[1] + 100 < temp[2]` ⟹ `100 + 100 < 500`, so `temp[2] = 200`.
   - `temp = [0, 100, 200, INT_MAX]`.

3. **Flight [0, 2, 500]**:
   - `dist[0] + 500 < temp[2]` ⟹ `0 + 500 < 200`, no update needed.

4. **Flight [2, 3, 100]**:
   - `dist[2] + 100 < temp[3]` ⟹ `500 + 100 < INT_MAX`, so `temp[3] = 600`.
   - `temp = [0, 100, 200, 600]`.

- **Update `dist` with `temp`**: `dist = [0, 100, 200, 600]`.

#### After `k+1 = 2` Iterations:

- `dist = [0, 100, 200, 600]`
- `dist[3] = 600`, so the minimum cost to reach city 3 from city 0 with at most 1 stop is `600`.

### Conclusion

The algorithm correctly identifies that the cheapest way to travel from city 0 to city 3 with at most 1 stop costs `600`. This dry run helps illustrate how the algorithm manages to find the shortest paths with constraints using the modified Bellman-Ford approach.
