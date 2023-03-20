# Best time to buy and sell problem

## Question
<https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/>

## Solution

```java
public class BestTimeToBuySellStock {
    public int maxProfit(int[] prices) {
        int currentPrice;
        int priceToBuy = prices[0];
        int priceToSell = prices[0];
        int maxProfit = 0;
        
        for (int price : prices) {
            currentPrice = price;

            if (currentPrice < priceToBuy) {
                priceToBuy = currentPrice;
                priceToSell = currentPrice;
            } else if (currentPrice > priceToSell) {
                priceToSell = currentPrice;
            }

            if (maxProfit < priceToSell - priceToBuy) {
                maxProfit = priceToSell - priceToBuy;
            }
        }
        return maxProfit;
    }
}
```