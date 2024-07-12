```cpp
#include <iostream>
#include <deque>

using namespace std;

int main() {
    deque<int> dq;

    // Insertion at the back
    dq.push_back(10);
    dq.push_back(20);
    dq.push_back(30);

    cout << "After push_back operations, deque contains: ";
    for (int i : dq) {
        cout << i << " ";
    }
    cout << endl;

    // Insertion at the front
    dq.push_front(5);
    dq.push_front(0);

    cout << "After push_front operations, deque contains: ";
    for (int i : dq) {
        cout << i << " ";
    }
    cout << endl;

    // Deletion from the back
    dq.pop_back();

    cout << "After pop_back operation, deque contains: ";
    for (int i : dq) {
        cout << i << " ";
    }
    cout << endl;

    // Deletion from the front
    dq.pop_front();

    cout << "After pop_front operation, deque contains: ";
    for (int i : dq) {
        cout << i << " ";
    }
    cout << endl;

    return 0;
}

```
