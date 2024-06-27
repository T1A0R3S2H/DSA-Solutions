# Method 1 (using isstringstream)

The `reverseWords` function is a method defined within the `Solution` class. It takes a single parameter `s`, which is a `std::string`, and returns another `std::string` representing the input string `s` with its words reversed in order.

## Function Breakdown

### 1. Initialization

- **`istringstream iss(s);`**: Initializes an `std::istringstream` object `iss` with the input string `s`. This allows treating `s` as a stream from which individual words can be extracted.

- **`vector<string> words;`**: Declares a vector `words` to store the individual words extracted from the string `s`.

- **`string word, reversedStr;`**: Declares two strings `word` to hold each word extracted and `reversedStr` to store the final reversed string.

### 2. Word Extraction

- **`while (iss >> word)`**: Uses a `while` loop to extract each word from the `iss` stream using the extraction operator (`>>`). Each extracted word is then appended to the `words` vector using `words.push_back(word);`.

### 3. Reversing the Words

- **`reverse(words.begin(), words.end());`**: Once all words are extracted and stored in `words`, the order of elements in the `words` vector is reversed using the `std::reverse` algorithm.

### 4. Constructing the Reversed String

- **`for (int i=0; i<words.size(); ++i)`**: Iterates through the `words` vector using a `for` loop. It constructs the `reversedStr` by concatenating each word from `words` with a space (`" "`) in between each pair of words.

- **`if (i > 0) { reversedStr += " "; }`**: Ensures that a space is added between words in `reversedStr`, except before the first word.

- **`reversedStr += words[i];`**: Concatenates the current word from `words` to `reversedStr`.

### 5. Return Statement

- **`return reversedStr;`**: Returns the `reversedStr`, which now contains the input string `s` with its words reversed.

## Code
```cpp
class Solution {
public:
    string reverseWords(string s) {
        istringstream iss(s);
        vector<string> words;
        string word, reversedStr;

        // Extract words from the string
        while (iss>>word) {
            words.push_back(word);
        }

        // Reverse the order of words
        reverse(words.begin(), words.end());

        // Construct the reversed string
        for (int i=0;i<words.size();++i) {
            if (i>0) {
                reversedStr+=" ";
            }
            reversedStr+=words[i];
        }

        return reversedStr;
    }
};
```

## Conclusion

The `reverseWords` function effectively reverses the order of words in a given string `s` by utilizing `std::istringstream` for word extraction, `std::reverse` for reversing the order of words in a vector, and simple string concatenation for constructing the final reversed string. It showcases efficient string manipulation techniques using standard C++ libraries.


# Explanation of `std::istringstream` in C++

## Introduction

In C++, `std::istringstream` is a class provided by the `<sstream>` header. It allows developers to treat strings as input streams, enabling efficient extraction and manipulation of data contained within strings.

## Key Features

### 1. String as a Stream

`std::istringstream` allows a `std::string` to be treated as a stream of characters. This capability is essential for tasks that involve parsing or extracting structured data from strings.

### 2. Usage Scenarios

- **Token Extraction**: Developers can use `std::istringstream` to extract individual tokens (words or numbers) from a string based on specified delimiters (e.g., spaces).
  
- **Data Conversion**: It simplifies the conversion of string representations of data types (such as integers, floating-point numbers) into their corresponding C++ types (`int`, `double`, etc.).

### 3. Example Usage

A typical use case involves initializing an `std::istringstream` object with a string and then using extraction operators (`>>`) to sequentially read and convert data from the stream.

### 4. Advantages

- **Simplicity**: `std::istringstream` provides a straightforward mechanism for handling string-based input operations.
  
- **Flexibility**: It supports various formats and types of data within a single string, enhancing its versatility for different applications.

## Conclusion

`std::istringstream` is a valuable component of C++'s standard library for processing string data as streams. Its ability to transform strings into input streams and facilitate efficient data extraction makes it an indispensable tool for tasks involving string parsing and data conversion.

