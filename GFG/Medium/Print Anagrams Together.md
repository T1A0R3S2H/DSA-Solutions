### Code (unorodered_map):
```cpp
class Solution {
  public:
    vector<vector<string>> anagrams(vector<string>& arr) {
        unordered_map<string, vector<string>> mp;
        
        for (auto x:arr) {
            string word=x;
            sort(word.begin(), word.end());
            mp[word].push_back(x);
        }
        
        vector<vector<string>> result;
        for (auto x:mp) {
            result.push_back(x.second);
        }
        return result;
    }
};
```

### Explanation:

1. **Sorting Each Word:**
   - Har word ko iterate karte hain `for` loop ke through.  
   - Ek copy bana kar har word ka sort karte hain (`sort(word.begin(), word.end());`).
   - Iska matlab hai ki agar ek word "eat" hai, toh sort karne ke baad woh "aet" ban jayega. Similarly, "tea" ya "ate" bhi "aet" ban jayega.  

2. **Mapping Sorted Words to Original Words:**
   - Ek `unordered_map` use kiya gaya hai, jo sorted word ko ek key ki tarah treat karta hai.  
   - Agar koi word us sorted form ke liye pehle se present hai, toh uske corresponding vector mein original word add kar dete hain (`mp[word].push_back(x);`).

3. **Grouping Anagrams:**
   - Finally, saare keys (`mp`) ko traverse karke har key ka value (jo ek vector hai) ek naya vector (`result`) mein push kar dete hain.

**Example Samajhiye:**
Input: `["eat", "tea", "tan", "ate", "nat", "bat"]`

1. Har word ko sort karte hain aur map mein store karte hain:
   ```
   "eat" -> "aet" -> mp["aet"] = ["eat"]
   "tea" -> "aet" -> mp["aet"] = ["eat", "tea"]
   "tan" -> "ant" -> mp["ant"] = ["tan"]
   "ate" -> "aet" -> mp["aet"] = ["eat", "tea", "ate"]
   "nat" -> "ant" -> mp["ant"] = ["tan", "nat"]
   "bat" -> "abt" -> mp["abt"] = ["bat"]
   ```

2. `unordered_map` ab kuch aisa lagta hai:
   ```
   "aet" -> ["eat", "tea", "ate"]
   "ant" -> ["tan", "nat"]
   "abt" -> ["bat"]
   ```

3. Final result mein bas `unordered_map` ke saare vectors ko push karte hain:
   ```
   Result = [["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]
   ```

**Short Summary:**
- Words ko sort karke ek common key banate hain.
- Jo words same sorted key ke hain, woh ek hi group mein (vector) store hote hain.
- Yehi group banakar output return karte hain.
