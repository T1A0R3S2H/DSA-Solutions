```cpp
int search(string pat, string txt) {
    unordered_map<char,int> freq;
    int ans = 0;
    
    for (auto& x : pat) {
        freq[x]++;
    }
    
    int count = freq.size();
    int k = pat.size();
    int i = 0, j = 0;
    
    while (j < txt.size()) {
        if (freq.find(txt[j]) != freq.end()) {
            freq[txt[j]]--;
            if (freq[txt[j]] == 0) {
                count--;
            }
        }
        
        if (j - i + 1 < k) {
            j++;
        } else if (j - i + 1 == k) {
            if (count == 0) {
                ans++;
            }
            if (freq.find(txt[i]) != freq.end()) {
                freq[txt[i]]++;
                if (freq[txt[i]] == 1) {
                    count++;
                }
            }
            i++;
            j++;
        }
    }
    return ans;
}
```
