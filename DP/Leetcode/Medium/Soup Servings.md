#### **Code:**

```cpp
class Solution {
public:
    unordered_map<int, unordered_map<int, double>> memo;

    double solveMem(int a, int b) {
        // Base cases
        if (a <= 0 && b <= 0) return 0.5; // A and B empty together
        if (a <= 0) return 1.0;           // A empty first
        if (b <= 0) return 0.0;           // B empty first

        // Check in memo
        if (memo[a][b] > 0) return memo[a][b];

        // Recursive formula for 4 operations
        memo[a][b] = 0.25 * (solveMem(a - 4, b) +
                             solveMem(a - 3, b - 1) +
                             solveMem(a - 2, b - 2) +
                             solveMem(a - 1, b - 3));
        return memo[a][b];
    }

    double soupServings(int n) {
        if (n >= 4800) return 1.0; // Converges to 1 for large n
        return solveMem((n + 24) / 25, (n + 24) / 25);
    }
};
```

---

#### **Explanation:**

1. **solveMem Function**:
   - Yeh recursive function hai jo soup A aur B ke remaining volumes ke basis par probability calculate karta hai ki soup A pehle khatam hoga, ya dono ek saath khatam honge.
   - **Base Cases**:
     - Agar dono soups (A aur B) khatam ho jaye, toh probability 0.5 hoti hai (dono ek saath khatam hote hain).
     - Agar soup A khatam ho jaye (bina B ke), toh probability 1.0 hogi (A pehle khatam hota hai).
     - Agar soup B khatam ho jaye (bina A ke), toh probability 0.0 hogi (B pehle khatam hota hai).
   - **Memoization**: 
     - `memo[a][b]` ko use karke hum already calculated values ko store karte hain taaki same values ke liye recursion na ho.
   - **Recursive Formula**: 
     - 4 operations (serve 100 ml, 75 ml, 50 ml, 25 ml) ko hum recursively evaluate karte hain aur unka average lete hain (har operation ki probability 0.25 hoti hai).

2. **soupServings Function**:
   - Agar \( n \) bohot bada ho (4800 ya usse zyada), toh hum directly return karte hain 1.0, kyunki jab \( n \) bohot zyada hota hai, toh soup A zyada waqt tak khatam hota hai aur probability 1 ho jati hai.
   - Agar \( n \) chhota ho, toh `solveMem` function ko call karte hain, lekin \( n \) ko scale karke `(n + 24) / 25` banate hain, jisse computation fast ho jata hai.

---

#### **Time Complexity**:
- **Time Complexity**: 
  - Recursive calls ko memoization ke through optimize kiya gaya hai, toh overall time complexity \( O(n^2) \) hogi, jahan \( n \) ka scale factor hai \( (n + 24) / 25 \). Isliye, recursion ki depth kaafi kam ho jati hai.
  
- **Space Complexity**:
  - Memoization table ke liye space \( O(n^2) \) hoga.

---

#### **Dry Run**:

1. **Input**: `n = 100`
   - Scale karte hain: \( \frac{100 + 24}{25} = 4 \).
   - Call `solveMem(4, 4)`.

2. **First Recursive Call** (`solveMem(4, 4)`):
   - Base case check: Na toh A khatam, na B khatam.
   - Call recursive functions for 4 operations:
     - `solveMem(0, 4)`
     - `solveMem(1, 3)`
     - `solveMem(2, 2)`
     - `solveMem(3, 1)`
   - In sabko recursively evaluate karke final probability calculate hoti hai.

3. **Final Output**: The result is returned after all recursive calls.

---

#### **Summary**:
- **Code**: Recursively solve karte hue, memoization use karte hain.
- **Time Complexity**: \( O(n^2) \).
- **Space Complexity**: \( O(n^2) \).
- **Dry Run**: Step-by-step recursive calls calculate karte hain probability.
