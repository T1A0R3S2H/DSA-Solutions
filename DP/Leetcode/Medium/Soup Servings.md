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

0. **NO INITIALISATION**:

**Initialization with `0` instead of `-1`:** Is question mein humne DP table ko **`unordered_map`** se initialize kiya hai, aur isme default value **`0`** hoti hai. Matlab agar koi state `(a, b)` compute nahi hui hai, toh vo automatically **`0`** return karegi. Iska matlab yeh hai ki **uncomputed states** ko track karne ke liye humne **`0`** ka use kiya hai. Ab **`0`** ek valid probability bhi ho sakti hai (jaise jab soup B khatam ho jaata hai), isliye humne **`-1`** ka use nahi kiya. Agar hum **`-1`** use karte, toh confusion ho sakta tha ki `-1` ka matlab state calculate nahi hui hai ya `0` ka matlab koi valid state hai. Isliye **`0`** ko default value banaya gaya hai, taki uncomputed states ko easily track kiya jaa sake bina kisi confusion ke.

**Why Not `-1`?** Usually, dusre DP problems mein hum **`-1`** se initialization karte hain, taaki hum easily check kar sakein ki koi state calculated hai ya nahi. Lekin is question mein **`unordered_map`** ka use kiya gaya hai, jo **default value** **`0`** return karta hai jab koi state computed nahi hoti. Agar hum **`-1`** use karte, toh **`0`** aur **`-1`** ke beech confusion hota, kyunki **`0.0`** ek valid probability ho sakti hai (jab B soup khatam ho jata hai), aur agar **`-1`** ko use karte, toh yeh state ke compute na hone ko indicate karne ka ek potential problem hota. Isliye humne **`0`** ko default value rakha hai, aur **unordered_map** ka advantage liya hai, jisme uncomputed states automatically **`0`** ke saath track hote hain, aur jise hum baad mein compute karke store kar lete hain.

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
