# Two Sum

## Question

In an array of integers find the indices of two elements such that their sum is equal to the given number.

## Constraints

* Assume only one solution
* Can't use the same number twice

Solution

```java
import java.util.HashMap;
import java.util.Map;

public class TwoSum {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> numberIndexMap = new HashMap<>(nums.length);

        for (int index = 0; index < nums.length; index++) {
            if (numberIndexMap.containsKey(target - nums[index])) {
                return new int[]{numberIndexMap.get(target - nums[index]), index};
            } else {
                numberIndexMap.put(nums[index], index);
            }
        }
        return new int[2];
    }
}
```