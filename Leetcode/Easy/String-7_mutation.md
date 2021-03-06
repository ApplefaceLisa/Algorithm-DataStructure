# [14. Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/description/)
## Problem Description
Write a function to find the longest common prefix string amongst an array of strings.

## Solution
If the common prefix exists, then indexOf(prefix) for every element should not be -1.
```
/**
 * @param {string[]} strs
 * @return {string}
 */
 var longestCommonPrefix = function(strs) {
    if (strs.length === 0) return "";
    
    let prefix = strs[0];
    
    for (let i = 1; i < strs.length; i++) {
        while (strs[i].indexOf(prefix) !== 0) {
            prefix = prefix.substring(0, prefix.length - 1);
        }
    }
    return prefix;
};
```

# [583. Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/description/) Medium
# [17. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/) Medium
