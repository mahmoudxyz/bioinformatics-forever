# Day1

## Roman to Integer

The problem can be found [here](https://leetcode.com/problems/roman-to-integer/description/)

## Solution one

Let's think, simple solution for this problem, will be change the way that system work, in another word, instead of making minus, will make everything just sum.


```python
class Solution:
    def romanToInt(self, s: str) -> int:
        roman = {
            "I": 1,
            "V": 5,
            "X": 10,
            "L": 50,
            "C": 100,
            "D": 500,
            "M": 1000
        }
        replace = {
            "IV": "IIII",
            "IX": "VIIII",
            "XL": "XXXX",
            "XC": "LXXXX",
            "CD": "CCCC",
            "CM": "DCCCC"
        }

        for k, v in replace.items(): 
            s = s.replace(k, v)
            
        return sum([roman[char] for char in s])
```

## Solution two

Another way to think about this, is just if we say smaller number before bigger number, we should minus, otherwise, we should continue adding numbers.

```python
class Solution:
    def romanToInt(self, s: str) -> int:
        roman = {
            "I": 1,
            "V": 5,
            "X": 10,
            "L": 50,
            "C": 100,
            "D": 500,
            "M": 1000
        }
        total = 0
        pre_value = 0

        for i in s:
            if pre_value < roman[i]:
                total += roman[i] - 2 * pre_value
            else:
                total += roman[i]
            
            pre_value = roman[i]
        
        return total
```

This solution in runtime beats 100%, but memory only 20% better

why I did this `roman[i] - 2 * pre_value`? because we need to minus the added value in the previous step.