[Link -- MUST WATCH](https://youtu.be/RrxpTWqj97A?si=jX9U3fp606Lz2Vml&t=902)

### **Code**:
```cpp
// creating a node that will store data and row and column of it
class node{
public:
    int data;
    int row;
    int col;

    node(int d, int r,int c){
        this->data = d;
        row = r;
        col = c;
    }
};

class cmp{
public:
    bool operator()(node* &a,node* &b){
        return a->data > b->data;
    }
};


class Solution {
public:
    vector<int> smallestRange(vector<vector<int>>& nums) {
        // create a min heap to store the data column and row of an element 
        priority_queue<node*,vector<node*>,cmp> pq;
        //declare initial range 
        int mini =  INT_MAX, maxi = INT_MIN;
        // push the first element of each list (min element of each list) in the heap and also update the min and max value
        for(int i = 0; i < nums.size(); i++){
            int val = nums[i][0];
            mini = min(mini,val);
            maxi = max(maxi,val);
            pq.push(new node(val,i,0));
        }
        // the range for all the first elements of the list will be [mini,maxi] 
        int start = mini, end = maxi;

        while(!pq.empty()){
            node* t = pq.top();
            pq.pop();

            mini = t->data;
            
            // if the new range is smaller than the previous one then update the new range 
            if(maxi - mini < end - start){
                start = mini;
                end = maxi;
            }
            else if(maxi - mini == end - start){
                if(mini < start){
                    start = mini;
                    end = maxi;
                }
            }

            // if next element exists in the list of mini
            if(t->col < nums[t->row].size() - 1){
                // update maxi if its greater than prev maxi
                maxi = max(maxi,nums[t->row][t->col+1]);
                // push the new element in the heap
                pq.push(new node(nums[t->row][t->col+1],t->row,t->col+1));
            }
            else{
                // if we have exhausted one of the lists then that means we have considered all possible ranges that will have atleast one member from each list
                break;
            }
        }
        return {start,end};
    }
};
```
### **Code Explanation**

1. **Node Class:**
   - Represents an element from one of the lists, with its value (`data`), row index (`row`), and column index (`col`).

2. **Comparator Class (`cmp`):**
   - Defines how to compare two `node` pointers. The priority queue uses this to maintain a min-heap based on the node values.

3. **Solution Class and `smallestRange` Function:**
   - **Initialization:**
     - A min-heap (`pq`) is used to keep track of the smallest elements from each list.
     - `mini` and `maxi` track the current minimum and maximum values across the lists.
     - The first element of each list is pushed into the heap.

   - **Processing the Heap:**
     - Extract the smallest element (`mini`) from the heap.
     - Update the range `[start, end]` if the new range is smaller or equal but has a smaller `mini`.
     - If possible, push the next element from the same list into the heap and update `maxi`.
     - Stop if any list is exhausted.

   - **Return the Range:**
     - Returns the smallest range `[start, end]`.

### **Time Complexity Analysis**

1. **Heap Operations:**
   - Each element is inserted into and removed from the priority queue (min-heap). In the worst case, there are \( n \cdot k \) elements (where \( k \) is the number of lists and \( n \) is the number of elements per list).
   - Each heap operation (insertion or extraction) takes \( O(\log(k)) \) time, where \( k \) is the number of lists.

2. **Overall Time Complexity:**
   - Given that there are \( O(n \cdot k) \) elements and each operation is \( O(\log(k)) \), the total time complexity is \( O((n \cdot k) \cdot \log(k)) \).

### **Space Complexity Analysis**

1. **Heap Space:**
   - The heap stores at most \( k \) nodes (one from each list).

2. **Auxiliary Space:**
   - Other than the heap, the space used for variables and results is \( O(1) \).

3. **Overall Space Complexity:**
   - The space complexity is \( O(k) \), where \( k \) is the number of lists.

### **Dry Run**

**Input:**
```cpp
vector<vector<int>> nums = {
    {1, 3, 5},
    {2, 6, 9},
    {4, 7, 8}
};
```

**Initialization:**
- `mini = INT_MAX`, `maxi = INT_MIN`
- Min-heap `pq` starts empty.

**Process:**

1. **Push Initial Elements:**
   - `nums[0][0] = 1`: `mini = 1`, `maxi = 1`, `pq` = `[1 (row 0, col 0)]`
   - `nums[1][0] = 2`: `maxi = 2`, `pq` = `[1, 2 (row 1, col 0)]`
   - `nums[2][0] = 4`: `maxi = 4`, `pq` = `[1, 2, 4 (row 2, col 0)]`

2. **First Iteration:**
   - Pop `1 (row 0, col 0)`, update `mini = 1`, `maxi = 4`
   - New range `[1, 4]` is valid. `start = 1`, `end = 4`
   - Push `3 (row 0, col 1)` into `pq`, `maxi = max(4, 3) = 4`
   - `pq` = `[2, 4, 3]`

3. **Second Iteration:**
   - Pop `2 (row 1, col 0)`, update `mini = 2`
   - New range `[2, 4]` is valid. `start = 2`, `end = 4`
   - Push `6 (row 1, col 1)` into `pq`, `maxi = max(4, 6) = 6`
   - `pq` = `[3, 4, 6]`

4. **Third Iteration:**
   - Pop `3 (row 0, col 1)`, update `mini = 3`
   - New range `[3, 6]` is valid. `start = 3`, `end = 6`
   - Push `5 (row 0, col 2)` into `pq`, `maxi = max(6, 5) = 6`
   - `pq` = `[4, 6, 5]`

5. **Fourth Iteration:**
   - Pop `4 (row 2, col 0)`, update `mini = 4`
   - New range `[4, 6]` is valid. `start = 4`, `end = 6`
   - Push `7 (row 2, col 1)` into `pq`, `maxi = max(6, 7) = 7`
   - `pq` = `[5, 6, 7]`

6. **Fifth Iteration:**
   - Pop `5 (row 0, col 2)`, update `mini = 5`
   - New range `[5, 7]` is valid. `start = 5`, `end = 7`
   - No more elements in row 0. Exit loop.

**Result:**
- The smallest range covering at least one element from each list is `[5, 7]`.
