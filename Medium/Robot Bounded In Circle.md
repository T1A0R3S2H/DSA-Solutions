### Code:
```cpp
class Solution {
public:
    bool isRobotBounded(string instructions) {
        int i = 0, j = 0; // Starting position (robot ka origin pe hona i.e., (0, 0))
        int dir = 0; // Direction ko 0 (North) se shuru kar rahe hain.
                     // dir = 0 -> North, dir = 1 -> East, dir = 2 -> South, dir = 3 -> West.

        for (char ch : instructions) { // Har instruction ko ek-ek karke process karenge.
            if (ch == 'G') { 
                // Agar 'G' hai, toh robot ko aage move karna hai. Ab direction ke hisaab se decide hoga ki kahan move karega:
                if (dir == 0) i++;          // Agar North (dir = 0) hai, toh y-axis pe ek step aage badhega.
                else if (dir == 1) j++;     // Agar East (dir = 1) hai, toh x-axis pe ek step aage badhega.
                else if (dir == 2) i--;     // Agar South (dir = 2) hai, toh y-axis pe ek step peeche jayega.
                else if (dir == 3) j--;     // Agar West (dir = 3) hai, toh x-axis pe ek step peeche jayega.
            } 
            else if (ch == 'L') { 
                // Agar 'L' hai, toh robot ko left (anti-clockwise) turn karwana hai.
                dir = (dir + 3) % 4; // Left turn ka matlab hai direction 1 step peeche hatega.
                                   // Modulo 4 isliye use kar rahe hain taaki direction 0-3 ke beech hi rahe.
            } 
            else if (ch == 'R') { 
                // Agar 'R' hai, toh robot ko right (clockwise) turn karwana hai.
                dir = (dir + 1) % 4; // Right turn ka matlab hai direction 1 step aage badhega.
                                   // Modulo 4 ensures ki direction valid rahe (0-3 ke beech).
            }
        }

        // Loop ke baad ab check karna hai ki robot circle mein hai ya nahi:
        // 1. Agar robot origin pe (i == 0 && j == 0) hai, toh woh circle mein bounded hai.
        // 2. Agar robot ka direction North (dir == 0) ke alawa kuch aur hai, tab bhi woh circle mein bounded hoga kyunki woh circular loop mein phir se origin pe aa jayega.
        return (i == 0 && j == 0) || dir != 0;
    }
};

```


### **Explanation:**

1. **Initial Setup:**
   - `x` and `y` represent the robot's position on the 2D plane. The robot starts at `(0, 0)`.
   - `dir` keeps track of the direction the robot is facing:
     - `0` = North
     - `1` = East
     - `2` = South
     - `3` = West

2. **Processing Instructions:**
   - For each instruction in the string:
     - If `G`:
       - Move one step in the current direction:
         - `dir == 0`: Move up (`y++`)
         - `dir == 1`: Move right (`x++`)
         - `dir == 2`: Move down (`y--`)
         - `dir == 3`: Move left (`x--`)
     - If `L`:
       - Turn **left** (anti-clockwise). This is done using `(dir + 3) % 4`, which shifts the direction backward in the cyclic order.
     - If `R`:
       - Turn **right** (clockwise). This is done using `(dir + 1) % 4`, which shifts the direction forward in the cyclic order.

3. **Final Check:**
   - After one pass through the instructions:
     - If `(x == 0 && y == 0)`: The robot has returned to the origin, so it is bounded.
     - If `dir != 0`: The robot is not facing North, which means repeating the instructions will cause it to move in a loop (circular motion).

---

### **Time Complexity:**

- **O(n)**, where `n` is the length of the instructions string. Each character is processed exactly once.

---

### **Space Complexity:**

- **O(1)**, as only a constant amount of extra space is used for variables `x`, `y`, and `dir`.

---

### **Dry Run:**

#### Input: `"GGLLGG"`

1. **Initial State:**  
   - `(x, y) = (0, 0), dir = 0 (North)`

2. **Processing Instructions:**  
   - `G`: Move North → `(x, y) = (0, 1)`
   - `G`: Move North → `(x, y) = (0, 2)`
   - `L`: Turn left → `dir = 3 (West)`
   - `L`: Turn left → `dir = 2 (South)`
   - `G`: Move South → `(x, y) = (0, 1)`
   - `G`: Move South → `(x, y) = (0, 0)`

3. **Final State:**  
   - `(x, y) = (0, 0), dir = 2 (South)`
   - Robot is bounded because it returned to the origin.

#### Output: `true`

---

#### Input: `"GG"`

1. **Initial State:**  
   - `(x, y) = (0, 0), dir = 0 (North)`

2. **Processing Instructions:**  
   - `G`: Move North → `(x, y) = (0, 1)`
   - `G`: Move North → `(x, y) = (0, 2)`

3. **Final State:**  
   - `(x, y) = (0, 2), dir = 0 (North)`
   - Robot is **not bounded** because it is moving straight and not looping.

#### Output: `false`

---

#### Input: `"GL"`

1. **Initial State:**  
   - `(x, y) = (0, 0), dir = 0 (North)`

2. **Processing Instructions:**  
   - `G`: Move North → `(x, y) = (0, 1)`
   - `L`: Turn left → `dir = 3 (West)`

3. **Final State:**  
   - `(x, y) = (0, 1), dir = 3 (West)`
   - Robot is bounded because it will loop back to the origin on repeated instructions.

#### Output: `true`
