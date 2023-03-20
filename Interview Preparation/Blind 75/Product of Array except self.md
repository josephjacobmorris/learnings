# Product of array except self

## Question

<https://leetcode.com/problems/product-of-array-except-self/description/>

## Solution

```java
public class ProductOfArrayExceptSelf {
    public int[] productExceptSelf(int[] nums) {
        int[] result = new int[nums.length];
        int numIndex;

        int[] productOfNumToRight = new int[nums.length];
        int productTillNow = 1;
        for (numIndex = nums.length - 1; numIndex >= 0; numIndex--) {
            productOfNumToRight[numIndex]= productTillNow;
            productTillNow *= nums[numIndex];
        }

        productTillNow = 1;
        for (numIndex = 0; numIndex < nums.length; numIndex++) {
            result[numIndex] = productOfNumToRight[numIndex] * productTillNow;
            productTillNow*= nums[numIndex];
        }
        return result;
    }
}
```