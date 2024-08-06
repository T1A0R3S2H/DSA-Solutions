```cpp
string FirstNonRepeating(string A) {
    // Array to keep track of character counts
    vector<int> count(26, 0);
    
    // Deque to maintain characters and their first appearance order
    deque<char> dq;
    
    string result = "";
    
    for (char c : A) {
        count[c - 'a']++;  // Increase count of current character
        
        // Add to deque if it's the first appearance
        if (count[c - 'a'] == 1) {
            dq.push_back(c);
        }
        
        // Remove from deque if it's no longer a first appearance character
        while (!dq.empty() && count[dq.front() - 'a'] > 1) {
            dq.pop_front();
        }
        
        // Determine result for current position
        if (!dq.empty()) {
            result += dq.front();
        } else {
            result += '#';
        }
    }
    
    return result;
}
```
