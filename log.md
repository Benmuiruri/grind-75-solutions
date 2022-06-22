### Challenge One - Two Sum

**State Assumptions / Clarification**

- The array must have 2 elements no need to check if array has at least two elements
- Assume that each input would have exactly one solution, that is there is ALWAYS only 1 pair of numbers that satisfy the condition
- You are allowed to return the answer (indices) in any order.

##### Possible Solutions.
1. Nested Loops 
   - Create two loops the outer loop will keep track of the first loop tracks the first number, the inner loop tracks the second number
   - **Time Complexity** is O(n^2), the worst case would is when the required combination is the last combination
   - **Space Complexity** is O(1)
2. **Optimal Solution** *Using a Hashmap*
   - Given an array [7,2,13,11,8], which two elements add up to 24.
   - Create a hashmap, iterate through each element in the given array, checking whether *required number = target - current number*. That is we check whether (24-7 = 17) the number 17 is in our hashmap if not, we add 7 and its index in hashmap {7:0}.
   - Next we check whether (24-2 = 22) the number 22 is in our hashmap if not, we add 7 and its index in hashmap {7:0}, {2:1}.
   - Next we check whether (24-13 = 11) the number 11 is in our hashmap if not, we add 13 and its index in hashmap {7:0}, {2:1}, {13:2}.
   - Next we check whether (24-11 = 13) the number 13 is in our hashmap .Because it is we know that the current number and 13 are the two numbers we are looking for, so we return the index of 13 and the index of the number we are currently at, that is (2,3).
   - **Time Complexity** is O(n)
   - **Space Complexity** is O(n)
  
##### Implementation
```
const twoSum = (nums, target) => {
  let seen = new Map();

  for (let i = 0; i <nums.length; i++) {
    const remaining = target - nums[i];
    if(seen.has(remaining)) {
      return [seen.get(remaining), i]
    } else {
       seen.set(nums[i],i);
    }
  }
};
```