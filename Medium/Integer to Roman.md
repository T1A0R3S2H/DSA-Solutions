## Method 1
The provided C++ code converts an integer to its Roman numeral representation by breaking the integer down into its thousands, hundreds, tens, and ones places. It uses four arrays, each representing the Roman numeral equivalents for digits from 0 to 9 at each place value (units, tens, hundreds, and thousands). The function `intToRoman` calculates the index for each place value by dividing or taking the modulus of the input number, and then accesses the corresponding Roman numeral string from the arrays. These strings are concatenated to form the final Roman numeral representation. For example, the number 3549 is broken down as 3000 (MMM), 500 (D), 40 (XL), and 9 (IX), resulting in the Roman numeral "MMMDXLIX". This approach leverages pre-defined mappings to efficiently and accurately construct the Roman numeral equivalent of any given integer.
```bash
class Solution {
public:
    string intToRoman(int num) {
        string ones[] = {"","I","II","III","IV","V","VI","VII","VIII","IX"};
        string tens[] = {"","X","XX","XXX","XL","L","LX","LXX","LXXX","XC"};
        string hrns[] = {"","C","CC","CCC","CD","D","DC","DCC","DCCC","CM"};
        string ths[]={"","M","MM","MMM"};
        
        return ths[num/1000] + hrns[(num%1000)/100] + tens[(num%100)/10] + ones[num%10];
    }
};
```
