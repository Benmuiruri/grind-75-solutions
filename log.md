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
   - *Sample input* nums = [2,7,11,15], target = 9
  
##### JS Implementation
```
const twoSum = (nums, target) => {
  let seen = {};

  for (let i = 0; i <nums.length; i++) {
    const remainder = target - nums[i];
    if(remainder in seen)) {
      return [seen[remainder], i]
    } else {
       seen[nums[i]] = i;
    }
  }
};
```

##### Ruby Implementation
```
def two_sum(nums, target)
  seen = {}
  nums.each_with_index do |num, index|
    remainder = target - num
    return [seen[remainder], index] if seen[remainder]
    seen[num] = index
  end
end
```

### Challenge Two - Best Time to Buy and Sell Stock

**State Assumptions / Clarification**

- The array can have 1 item, in that case, return 0 because we dont have a day we can sell the stock
- We must always return something, either 0 if it is not possible to make a profit, or return the profit obtained from buying and selling the stock

##### Possible Solutions.
1. **Optimal Solution** Sliding window (two-pointer technique)
   - Sliding window is a possible technique because we want to calculate the profit from subsections of the array. Therefore, to do it in linear time, we use two pointers that will be initialized from [0] and [1] and slide through the array based on our condition.
   - Initialize maximum profit as 0. If it is not possible to make profit, we shall return that value. 
   - Set a left pointer at index 0, and set a right pointer at index 1. 
   - inside a while loop, we will check if the right pointer is within the bounds of the array length we do the following
     - We check if left pointer is less than right pointer, if it is, we calculate the profit by subracting the value at left pointer from value of right pointer. then we update max_profit by checking which is the larger value between current max_profit and the profit we just calculated
     - Else, if the left pointer is greater than right pointer, we know that the right pointer will be new minimum so we update the left pointer to that current position and update the right pointer to the new left + 1.
   - Then we loop again, until right pointer has reached the end of the array, at which point we return the max_profit.
   - **Time Complexity** is O(n) because we search through the array once.
   - **Space Complexity** is O(1)
   - *Sample input* [7,1,5,3,6,4]
  
##### JS Implementation
```
var maxProfit = function(prices) {
let max_profit = 0
let left = 0
let right = 1

while (right < prices.length) {
  if (prices[left] < prices[right]) {
    let profit = prices[right] - prices[left]
    max_profit = Math.max(max_profit, profit);
  } else {
    left = right
  }
  right++;
}
return max_profit
};
```

##### Ruby Implementation
```
def max_profit(prices)
  return 0 if prices.count < 2

  profit = 0
  min_price = prices[0]

  (1..prices.count-1).each do |k|
    profit    = prices[k] - min_price if profit < prices[k] - min_price
    min_price = prices[k]             if prices[k] < min_price
    end
  profit
end
```