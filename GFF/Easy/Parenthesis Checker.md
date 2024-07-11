```cpp
class Solution
{
    public:
    //Function to check if brackets are balanced or not.
    bool ispar(string x)
    {
        stack<char> st;
        for (char c : x) {
            if (c == '(' || c == '{' || c == '[') {
                st.push(c);
            }

            else {
                if (st.empty() || (c == ')' && st.top() != '(') ||
                    (c == '}' && st.top() != '{') ||
                    (c == ']' && st.top() != '[')) {
                    return false;
                }

                else {
                    st.pop();
                }
            }
        }
        return st.empty();
    }

};
```
