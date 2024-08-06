# Method 1 (using stack)
```cpp
class Solution
{
    public:
    queue<int> rev(queue<int> q)
    {
        stack <int> s;
        while (!q.empty()){
            s.push(q.front());
            q.pop();
        }
        while (!s.empty()){
            q.push(s.top());
            s.pop();
        }
        return q;
    }
};
```

# Method 2 (recursion)
```cpp
class Solution
{
    public:
      queue<int> rev(queue<int> q)
      {
          queue <int> q2;
          revQueue(q, q2);
          return q2;
      }
    private:
      void revQueue(queue<int>&q1, queue<int>&q2){
        if (q1.empty()) return;
        int element=q1.front();
        q1.pop();
        revQueue(q1, q2);
        q2.push();
      }
};
```
