```cpp
void heapify(vector<int>& nums, int n, int i) {
        int largest=i;
        int left=2*i+1;
        int right=2*i+2;
        if (left<n && nums[left]>nums[largest]) {
            largest=left;
        }
        if (right<n && nums[right]>nums[largest]) {
            largest=right;
        }
        if (largest!=i) {
            swap(nums[i], nums[largest]);
            heapify(nums, n, largest);
        }
    }
```
