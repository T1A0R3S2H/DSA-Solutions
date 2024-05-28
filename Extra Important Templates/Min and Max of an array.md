```
#include <iostream>
#include <climits> // For INT_MIN and INT_MAX

// Function to find the minimum element in an array
int findMin(const int arr[], int size) {
    if (size <= 0) {
        std::cerr << "Error: Array is empty or invalid size." << std::endl;
        return INT_MIN; // Indicate an error with INT_MIN
    }

    int minVal = INT_MAX; // Start with a very large value
    for (int i = 0; i < size; i++) {
        if (arr[i] < minVal) {
            minVal = arr[i];
        }
    }
    return minVal;
}

// Function to find the maximum element in an array
int findMax(const int arr[], int size) {
    if (size <= 0) {
        std::cerr << "Error: Array is empty or invalid size." << std::endl;
        return INT_MAX; // Indicate an error with INT_MAX
    }

    int maxVal = INT_MIN; // Start with a very small value
    for (int i = 0; i < size; i++) {
        if (arr[i] > maxVal) {
            maxVal = arr[i];
        }
    }
    return maxVal;
}

int main() {
    int numbers[] = {5, 2, 9, 1, 7};
    int size = sizeof(numbers) / sizeof(numbers[0]);

    int minVal = findMin(numbers, size);
    int maxVal = findMax(numbers, size);

    std::cout << "Minimum value: " << minVal << std::endl;
    std::cout << "Maximum value: " << maxVal << std::endl;

    return 0;
}
```
