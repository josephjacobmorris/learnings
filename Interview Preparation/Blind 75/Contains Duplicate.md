# Contains Duplicate

## Question

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class ContainsDuplicate {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> numbersSoFar = new HashSet<Integer>();

        for (int num : nums) {
            if (!numbersSoFar.add(num)) {
                return true;
            }
        }
        return false;
    }
}
```