### Code:
```cpp
class Solution {
public:
    // Function to detect if there is a cycle in the graph
    bool isCyclic(int V, vector<int> adj[]) {
        vector<int> inedge(V, 0);
        // Calculate in-degrees of all vertices
        for (int i=0; i<V; i++) {
            for (auto j:adj[i]) {
                inedge[j]++;
            }
        }
        queue<int> q;
        for (int i=0; i<V; i++) {
            if (inedge[i]==0) {
                q.push(i);
            }
        }
        int count=0;
        while (!q.empty()) {
            int topNode=q.front();
            q.pop();
            count++;
            // Decrease the in-degree of adjacent vertices
            for (auto i:adj[topNode]) {
                inedge[i]--;
                if (inedge[i]==0) {
                    q.push(i);
                }
            }
        }
        // If count is equal to V, no cycle is present, else a cycle is present
        return count!=V;
    }

    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<int> adj[numCourses];
        vector<int> inedge(numCourses, 0);
        vector<int> result; // To store the course order
        for (auto& pre : prerequisites) {
            adj[pre[1]].push_back(pre[0]);
            inedge[pre[0]]++; // Increase the in-degree of the course
        }
        queue<int> q;
        for (int i=0; i<numCourses; i++) {
            if (inedge[i]==0) {
                q.push(i);
            }
        }
        // Process the courses in topological order
        while (!q.empty()) {
            int course=q.front();
            q.pop();
            result.push_back(course);
            // Decrease the in-degree of neighboring courses
            for (auto& neighbor:adj[course]) {
                inedge[neighbor]--;
                // If a course has no remaining prerequisites, add it to the queue
                if (inedge[neighbor]==0) {
                    q.push(neighbor);
                }
            }
        }
        
        // If the result contains all the courses, return it, else return an empty array (cycle detected)
        if (result.size()==numCourses) {
            return result;
        }
        else {
            return {};
        }
    }
};
```
#### **Explanation:**

The provided code solves the "Course Schedule II" problem using Kahn's Algorithm for topological sorting in a Directed Acyclic Graph (DAG). The algorithm determines if all courses can be completed given their prerequisites and returns a valid order of courses.

1. **isCyclic Function:**
   - **Purpose:** Detects if there is a cycle in the graph.
   - **Approach:**
     - Calculates the in-degrees (number of incoming edges) for each vertex.
     - Uses a queue to perform a topological sort by adding vertices with in-degree 0 (no prerequisites) and then processing their neighbors.
     - If not all vertices are processed (`count != V`), a cycle is present.

2. **findOrder Function:**
   - **Purpose:** Returns a valid order of courses or an empty array if it's impossible to finish all courses (cycle detected).
   - **Approach:**
     - Constructs the adjacency list from the prerequisites.
     - Calculates in-degrees for each course.
     - Uses a queue to perform Kahn's Algorithm to get a topological order of courses.
     - Checks if the result includes all courses; if not, it implies a cycle.

#### **Time Complexity:**
- **isCyclic Function:** `O(V + E)`
  - **V:** Number of vertices (courses).
  - **E:** Number of edges (prerequisites).
  - Calculating in-degrees and processing vertices with the queue are both linear in terms of vertices and edges.

- **findOrder Function:** `O(V + E)`
  - Constructing the adjacency list and processing the topological sort both have linear time complexity with respect to vertices and edges.

#### **Space Complexity:**
- **isCyclic Function:** `O(V)`
  - Space used for in-degrees and the queue is linear in the number of vertices.

- **findOrder Function:** `O(V + E)`
  - Space used for adjacency list, in-degrees, and the result vector is linear with respect to vertices and edges.

#### **Dry Run:**

**Example:**
`numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]`

1. **Adjacency List Construction:**
   - `adj[0] = [1, 2]`
   - `adj[1] = [3]`
   - `adj[2] = [3]`
   - `adj[3] = []`
   - `inedge = [0, 1, 1, 2]`

2. **Queue Initialization:**
   - Start with courses with in-degree 0: `q = [0]`

3. **Topological Sort Execution:**
   - Dequeue `0`, add to result: `result = [0]`
   - Decrease in-degrees of neighbors `[1, 2]`: `inedge = [0, 0, 0, 2]`
   - Enqueue `1` and `2`: `q = [1, 2]`
   - Dequeue `1`, add to result: `result = [0, 1]`
   - Decrease in-degree of neighbor `[3]`: `inedge = [0, 0, 0, 1]`
   - Dequeue `2`, add to result: `result = [0, 1, 2]`
   - Decrease in-degree of neighbor `[3]`: `inedge = [0, 0, 0, 0]`
   - Enqueue `3`: `q = [3]`
   - Dequeue `3`, add to result: `result = [0, 1, 2, 3]`

4. **Final Check:**
   - Result size equals `numCourses`, so return `[0, 1, 2, 3]` or any valid topological order.
