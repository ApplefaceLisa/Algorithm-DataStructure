## 1. Two Sum
### Problem Description
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

### Solution
O(n) solution:
Maintainance a map, store `(target - element) : element_index` for each element. Traverse input array, for each element if (target - element) existing, return index.

```
var twoSum = function(nums, target) {
    var l = nums.length;
    var map = {};
    for (var i = 0; i < l; i++) {
        var cur = nums[i];
        var tgt = target - cur;
        var j = map[cur];
        if (map[cur] !== undefined) {
            return [j, i];
        }
        map[tgt] = i;   // store current element's target and current index
    }
    return [];
}
```

Using Javascript built-in Map implement:
```
var twoSum = function(nums, target) {
    var myMap = new Map();
    var res = [];
    nums.some(function(val, index) {
        var num = target - val;
        if (myMap.has(val)) {
            res = [myMap.get(val), index];
            return true;
        }
        myMap.set(num, index);
    });
    return res;
}
```

## Knowledge Used
#### [Javascript built-in objects : Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)
- `var myMap = new Map([iterable])`

Properties | Description
---------- | ------------------------
Map.prototype.size | Returns the number of key/value pairs in the Map object.

Methods | Description
------- | ---------------------------
Map.prototype.set(key, value) | Sets the value for the key in the Map object. Returns the Map object.
Map.prototype.get(key) | Returns the value associated to the key, or undefined if there is none.
Map.prototype.delete(key) | Removes any value associated to the key and returns the value that Map.prototype.has(key) would have previously returned. Map.prototype.has(key) will return false afterwards.
Map.prototype.clear() | Removes all key/value pairs from the Map object.
Map.prototype.has(key) | Returns a boolean asserting whether a value has been associated to the key in the Map object or not.
Map.prototype.keys() | Returns a new Iterator object that contains the keys for each element in the Map object in insertion order.
Map.prototype.values() | Returns a new Iterator object that contains the values for each element in the Map object in insertion order.
Map.prototype.entries() | Returns a new Iterator object that contains an array of [key, value] for each element in the Map object in insertion order.
Map.prototype.forEach(callbackFn[, thisArg]) | Calls callbackFn once for each key-value pair present in the Map object, in insertion order. If a thisArg parameter is provided to forEach, it will be used as the this value for each callback.
Map.prototype[@@iterator]() | Returns a new Iterator object that contains an array of [key, value] for each element in the Map object in insertion order.