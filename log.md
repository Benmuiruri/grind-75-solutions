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
2. **Optimal Solution** _Using a Hashmap_
   - Given an array [7,2,13,11,8], which two elements add up to 24.
   - Create a hashmap, iterate through each element in the given array, checking whether _required number = target - current number_. That is we check whether (24-7 = 17) the number 17 is in our hashmap if not, we add 7 and its index in hashmap {7:0}.
   - Next we check whether (24-2 = 22) the number 22 is in our hashmap if not, we add 7 and its index in hashmap {7:0}, {2:1}.
   - Next we check whether (24-13 = 11) the number 11 is in our hashmap if not, we add 13 and its index in hashmap {7:0}, {2:1}, {13:2}.
   - Next we check whether (24-11 = 13) the number 13 is in our hashmap .Because it is we know that the current number and 13 are the two numbers we are looking for, so we return the index of 13 and the index of the number we are currently at, that is (2,3).
   - **Time Complexity** is O(n)
   - **Space Complexity** is O(n)
   - _Sample input_ nums = [2,7,11,15], target = 9

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

_Input: nums = [7,1,5,3,6,4]_

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

### Challenge Three - Product of Array Except Self

_Input: nums = [1,2,3,4]_

**State Assumptions / Clarification**

- The array must always have at least 2 elements so no need to check if array is empty.
- If array has two elements, the product result will be the same array in reverse.

##### Possible Solutions.

1. **Optimal Solution**
   - First we create a `result` array that we shall return
   - Then we initialize `product_from_left` as 1 and `product_from_right` as 1
   - In a for loop, we iterate the `nums array` from `i = 0` to `i = 3` doing 2 actions
     - First we set `result[i]` as `product_from_left`
     - Then we update `product_from_left` to be the value of `num[i]` x current `product_from_left`
       - *That means in our first iteration, `result[0]` will be 1, and our updated `product_from_left` will be `1*1`\*
       - *In our second iteration, `result[1]` will be 1 and our updated `product_from_left` will be 2*1\*
     - After the first loop our result array will have `[1,1,2,6]`
   - Then in a second for loop we iterate the `nums array` from `i = 3` to `i = 0` doing 2 actions
     - First we set `result[i]` as `product_from_right` \* `result[i]`(remember we are setting `results[3]` this time)
     - Then we update our `product_from_right` to be the value of `nums[i]` \* current `product_from_right`
       - _That means in our first iteration `result[3]` will be `1 _ 6`* and our updated `product_from_right`will be`4\*1`
       - _in our second iteration `result[2]` will be `4 _ 2`* and our updated `product_from_right`will be`3*4`*
     - After the second loop our result array will have `[24,12,8,6]`
   - After the second loop, we return the `result` array
   - **Time Complexity** is O(n)
   - **Space Complexity** is O(1)

##### JS Implementation

```
var productExceptSelf = function(nums) {
   let leftProduct = 1, rightProduct = 1;
    let res = [new Array(nums.length)];
    for (let i = 0; i < nums.length; i++) {
        res[i] = leftProduct;
        leftProduct *= nums[i];
    }
    for (let i = nums.length-1; i >= 0; i--) {
        res[i] *= rightProduct;
        rightProduct *= nums[i];
    }
    return res
};
```

### Challenge Four - Valid Anagram

_Input: s = "anagram", t = "nagaram"_

**State Assumptions / Clarification**

- Inputs `s` and `t` consist of lowercase English letters.
- Input `s` is at least 1 character (which will be always be true)
- It is not stated that `s` and `t` are same length so first check that and if they are not return `false`.

#### Possible Solutions.

1.  **Possible Optimal solution** _Using a Hash Map to store the characters of s then compare whether characters of t match what is in the Hash_

- This solution avoids adding `log(n)` time needed to sort, hence why it is optimal.
- First check if `s.length === t.length`, if not return false.
- If they are same length, let's check if they are anagrams. First initialize an empty `counts` hash map
- Inside a for loop that populates the `counts` hash.
  - Inside the for loop you eiher add counts[c] as 0 or you add a 1 to the the existing count
- Inside a second for loop. check whether all characters in `t` are in in counts[c], if not return false.
- Otherwise return false.

- **Time Complexity** is O(n)
- **Space Complexity** is O(1)

2. **Easiest solution** Use JS built in methods (`split`, `sort`, and `join` )

   - First check if `s.length === t.length`, if not return false.
   - split the string, then `sort` the the characters, then join the characters.... Then return the comparison of the two using ===. It will be either `true` or `false`.
   - However, this easy solution is slower by `log(n)` time because the `sort` method had to traverse the array created by `split`.

   - **Time Complexity** is O(n log(n))
   - **Space Complexity** is O(1)

##### JS Implementation

```
var isAnagram = function(s, t) {
  if (t.length !== s.length) return false;
  return s.split('').sort().join('') === t.split('').sort().join('')
}
};
```

```
var isAnagram = function(s, t) {
  if (t.length !== s.length) return false;
  let count = {};
  for (c of s) {
    count[c] = (count[c] || 0) + 1;
  }
  for (c of t){
    if (!count[c]) return false;
    count[c]--; // If we find a character we decrease it's count.
  }
  return true.
}
};
```

### Challenge Five - Merge Two Sorted Lists

**State Assumptions / Clarification**

- The inputs are not an array, they are linked lists.
- The two inputs given will always be sorted so no need to sort.
- It is possible that both linked lists are of different sizes
- It is possible to get an empty list for one of both lists.

##### Possible Solutions.

1. _Iterative approach_

   - This is a more _beginner_ implementation, but it is easier for me to wrap my head around it, than the recursive solution.
   - First, initialise a dummy `ListNode`, this is what will be returned `let newList = new ListNode(0)`
   - Then create a `headOfNewList` to maintain a reference to the head of the `newList`, since we shall return what is next of the head.
   - Now we enter into a `while` loop to compare the values. While both lists contain elements `(l1 != null && l2 != null)`
     - If `l1`'s value is smaller than `l2`'s value, assign `newList.next` as `l1` and update `l1` to `l1.next`
     - Else `l2`'s value is smaller than `l2`'s value, assign `newList.next` as `l2` and update `l2` to `l2.next`
   - After perfroming either of the blocks, update `newList` to be` newList.next` for the next iteration
   - Outside of the while loop you have two if check's `l1 == null` it means there are no more elements in l1, so we assign all that is remaining in l2 to newList.next .. `newList.next = l2`
   - Another if check's l2 == null it means there are no more elements in l12 so we assign all that is remaining in l1 to newList.next .. `newList.next = l1 `
   - Finally, after running the while loop, and the if checks, we have finished updating our newList and we can now return `headOfNewList.next`;

   - **Time Complexity** is O(n+m), the worst case would is when the required combination is the last combination
   - **Space Complexity** is O(1)

2. _Recursive approach_
   - As with any recursive approach, first set up the base cases. The base cases will be if `l1` is `null` return `l2`, and if `l2` is `null` return `l1`
   - For the recursion itself, we are going to run an if..else block.
     - If l1.val is less than l2.val, move on to l1.next by setting it as the calling the the function but this time passing in the l1.next, and the same node from l2. Then return l1
     - Else l2.val is less than l1.val, move on to l2.next by setting it as the calling the the function but this time passing in the l2.next, and the same node from l1. Then return l1

- By the end of the recursion You will have your answer.

  - **Time Complexity** is O(n + m) - At every call of recursion, we are adding one node to the output.
  - **Space Complexity** is O(n + m)

##### JS Implementation

```
var mergeTwoLists = function (l1, l2) {
    if (l1 == null) return l2;
    if (l2 == null) return l1;

    if (l1.val < l2.val) {
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
    }
    else {
        l2.next = mergeTwoLists(l1, l2.next);
        return l2;
    }
};
```

```
var mergeTwoLists = function (l1, l2) {

    let newList = new ListNode(0);
    let headOfNewList = newList

    while (l1 != null && l2 != null) {

        if (l1.val < l2.val) {
            newList.next = l1;
            l1 = l1.next;
        } else {
            newList.next = l2;
            l2 = l2.next;
        }
        newList = newList.next;
    }

    if (l1 == null) {
        newList.next = l2;
    } else {
        newList.next = l1;
    }

    return headOfNewList.next;
};
```

### Challenge Six - Valid Palindrome

_Input: s = "A man, a plan, a canal: Panama"_

**State Assumptions / Clarification**

- The input is not lowercase, so you need to call `lowercase()` on the input.
- If the string provided is a sting with a space, once you remove all alphanumeric characters, which includes spaces, you are left with an empty string

##### Possible Solutions.

1. Simple Solution

   - The straight forward solution is first to define a regex
   - Second remove all non alphanumeric characters
   - Then reverse the string by using the `split()`, `reverse()` and `join()` methods
   - **Time Complexity** is O(n)
   - **Space Complexity** is O(3n) because you need space to create the result of `split()` and `reverse()`

2. **Optimal Solution**

   - This solution avoids using built in methods, reverse and join.
   - It also shows something interesting, `you can initialize multiple variables to use in your for loop`.
   - First step in the solution is to lowercase the input and use regex to remove alphanumeric characters.
   - Next in a `for loop`, initialize two variables,` i==; j = str.length,` then condition is as long as `i<=j` increment `i++` and decrement `j--`
   - inside the for loop `if charAt(i) !== charAt(j) return false` since it is not a palindrome.
   - Otherwise if all are equal return true outside the `for loop` because we have checked all characters.

   - **Time Complexity** is O(n)
   - **Space Complexity** is O(1)

##### JS Implementation

```
var isPalindrome = function(s) {
    let regEx = /[^A-Za-z0-9]/g;
    let str = s.toLowerCase().replace(regEx, '');
    let reverseStr = str.split('').reverse().join('');
    return str === reverseStr;
};
```

_Optimal_

```
var isPalindrome = function(s) {
    let regEx = /[^A-Za-z0-9]/g;
    let str = s.toLowerCase().replace(regEx, '');
    for (let i=0; j = str.length -1; i <=j; i++, j--) {
      if(s.charAt(i) !== s.charAt(j)) return false
    }
    return true;
};
```

### Challenge Seven - Longest Substring Without Repeating Characters

_Input: s = "abcabcbb"_

**State Assumptions / Clarification**

- It is possible for a string to be of 0 length. In that case, if string is of length 0 then the longest substring is 0
- The string can also contain numbers or spaces, therefore for a string 'abcd1' the length of longest substring is 4

##### Possible Solutions.

1. **Optimal Solution** Sliding window (two-pointer technique)
   - Sliding window is a possible technique because we want to keep track of the longest substring as we move from left to right.
   - We shall use a set to keep track of the substring.
   - First, we initialize a left pointer at `index [0]` of the index and a right pointer at `index [0]` of the string.
   - Next we initialize the` maxlength as 0`, we shall be updating this `maxlength` depending on the size of the set, and in the end we shall return that `maxlength`.
   - Inside a while loop, `while (right < s.length)`
     - if `(!set.has(s[right]))` it means we have not seen the character so we add it to our set `set.add(s[right])`, increment `right++` so that we check the character in the next `index` and update `maxlength` with the new length of set. `maxLength = Math.max(maxLength, set.size)`
     - Else it means that set has that character so we delete the character from set `set.delete(s[left])` and update `left++`.
   - Finally, outside the while loop, after we have checked all characters of the string, we return `maxLength` which we were updating inside the loop.
   - **Time Complexity** is O(n) because we passed through the string only once.
   - **Space Complexity** is O(1)

##### JS Implementation

```
var lengthOfLongestSubstring = function(s) {
  if (!s.length) return 0;

  const set = new Set();
  let left = 0;
  let right = 0;
  let maxLength = 0;

  while (right < s.length) {
    if (!set.has(s[right])) {
      set.add(s[right]);
      right++;

      maxLength = Math.max(maxLength, set.size);
    } else {
      set.delete(s[left]);
      left++;
    }
  }
  return maxLength;
}
```

### Challenge Eight - Valid Parentheses

_Input: s = "()"_

**State Assumptions / Clarification**

- The minimum length of a string is 1 which in that case you return `false`.
- The string s contains only parenthesis

##### Possible Solutions.

1. **Optimal Solution** Using a s`tack data structure` to store paranthesis
   - First we check if the input string has 1 or 0 characters we return `false` immediately
   - Second we check if the input string is odd in length, then we return `false` immediately
   - Otherwise we initialize our empty `stack`.
   - Then in a `for loop`, we traverse through the string characters.
     - When we encounter an opening bracket we push the closing bracket into the stack.
     - When we encounter a closing bracket we pop the last element to enter the stack and expect it to be the opening bracket.
     - For instance, if we have a string `()`, we push `(` into the stack then when we encounter the `)` we pop from the stack expecting a `(` and that would be a valid parenthesis.
     - However, if we have a string `(}`, we push `(` into the stack and when we encounter `}` we would pop from the stack, expecting `{` hence it would be an invalid parenthesis.
   - Outside the for loop, after checking all characters, we expect `stack.length === 0` to be true if the string was valid and false if string was not valid
   - **Time Complexity** is O(n) because we passed through the string only once.
   - **Space Complexity** is O(1)

##### JS Implementation

```
var isValid = function(s) {
    if (s.length < 2) return false
    if (s.length % 2 !== 0) return false
    const stack = [];

    for (let i = 0 ; i < s.length ; i++) {
        let c = s.charAt(i);
        switch(c) {
            case '(': stack.push(')');
                break;
            case '[': stack.push(']');
                break;
            case '{': stack.push('}');
                break;
            default:
                if (c !== stack.pop()) {
                    return false;
                }
        }
    }
    return stack.length === 0;
};
```

### Challenge Nine - Ransom Note

_Input: ransomNote = "a", magazine = "b"_

**State Assumptions / Clarification**

- The length of ransomNote and magazine will always be greater than or equal to 1 so no need to check if either is 0
- Both ransomNote and magazine consist of lowercase English letters.

##### Possible Solutions.

1. **Optimal Solution**

   - First we create an empty `lookup` hashmap
   - Then we loop through the letters of magazine while updating `lookup` in the end we have something like { a:1, b:1 }
   - In a second for loop we check if the any of the letters of ransomNote are not in lookup, we return false, otherwise we decrement the count of letter.
   - In the event all letters of ransomNote are in magazine, we will decrement the counter of each letter until all letters are accounted for and outside the for loop we return `true`

   - **Time Complexity** is O(n)
   - **Space Complexity** is O(n)

##### JS Implementation

```
var canConstruct = function(ransomNote, magazine) {
    const lookup = {};
    for(let letter of magazine) {
          lookup[letter] ? lookup[letter]++ : lookup[letter]=1
    }
    for(let letter of ransomNote) {
        if(!lookup[letter]) {
            return false;
        }
        lookup[letter]--;
    }
    return true;
};
```

### Challenge Ten - Generate Parentheses

_Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses._
_Input: n = 3_
_Output: ["((()))","(()())","(())()","()(())","()()()"]_

**State Assumptions / Clarification**

- n is always greater than 1. In the event n is 1 return `["()"]` since n represents a pair of `()` of parenthesis

##### Possible Solutions.

1. **Optimal Solution - Recursion**

   - Please refer to [Bangar](https://leetcode.com/problems/generate-parentheses/discuss/1402685/Javascript-easyandclean-solution) whose solution helped me get unstuck.
   - First we define an `empty result array`, which is what we shall return.
   - Next we define a recusive function that an empty sring- temp, left, and right => `const backtracking = (temp, left, right) => { }`
   - Inside the recursive function, the base case is if (left === 0 && right === 0) then push and empty string and return. 
   - Now for the rest of the recursive function, 
     - if left > 0, call backtracking with (temp + '(', left-1, and right). Left decrements because we have added a left.
     - if right > 0, call backtracking with (temp + '(', left, and right-1). Right decrements because we have added a right. 
   - Now that we have defined out backtracking function, we call it with ('', n,n)
   - Eventually we will return result. When left and right are 0 and we have our updated temp string inside result array like `[()]`

   - **Time Complexity** is O(n log n) 
   - **Space Complexity** is O(n)

##### JS Implementation

```
var generateParenthesis = function(n) {
  let result = []

    function backtracking(temp, left, right) {
        if(left == 0 && right == 0) {
           result.push(temp)
            return
        }
    
        if(left > 0) {
            backtracking(temp + '(', left-1, right)
        }
    
        if(right > left) {
            backtracking(temp + ')', left, right-1)
        }
    }
    backtracking('', n, n)
    
    return result
};
```
