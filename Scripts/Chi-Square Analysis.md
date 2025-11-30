# **Statistical Test: Chi-Square Analysis**

## Test 1: Late Delivery vs Repeat Purchases
**SQL Query - Extract Contingency Table:**
```sql
-- Export this data for chi-square test
SELECT 
    CASE WHEN is_late = 1 THEN 'Late' ELSE 'OnTime' END as delivery_status,
    CASE WHEN is_repeat_customer = 1 THEN 'Repeat' ELSE 'OneTime' END as customer_type,
    COUNT(DISTINCT customer_unique_id) as count
FROM (
    SELECT DISTINCT ON (customer_unique_id)
        customer_unique_id,
        is_late,
        is_repeat_customer,
        order_purchase_timestamp
    FROM summary
    WHERE order_status = 'delivered'
    ORDER BY customer_unique_id, order_purchase_timestamp
) first_orders
GROUP BY 
    CASE WHEN is_late = 1 THEN 'Late' ELSE 'OnTime' END,
    CASE WHEN is_repeat_customer = 1 THEN 'Repeat' ELSE 'OneTime' END;
```
**Query Results:**

<img width="308" height="140" alt="Screenshot 2025-11-30 at 4 48 25 PM" src="https://github.com/user-attachments/assets/38f6dc88-f914-453f-bad3-a0bc66ba7cd4" />


**Python - Chi-Square Test:**
```python
from scipy.stats import chi2_contingency

observed = [[163, 6183], [2816, 84196]]
chi2, p_value, dof, expected = chi2_contingency(observed)

print(f"Chi-square statistic: {chi2}")
print(f"P-value: {p_value}")
print(f"Conclusion: {'Significant' if p_value < 0.05 else 'Not significant'}")
```
**Test Results:**

<img width="434" height="116" alt="Screenshot 2025-11-30 at 4 50 30 PM" src="https://github.com/user-attachments/assets/a91acda2-6d04-4efa-97c7-55765cf4e114" />

- **Null hypothesis:** Delivery timing and repeat purchases are independent
- **Chi-square** = 8.32, p = 0.0039 → Statistically significant
- **Effect size:** 0.67 percentage points (2.57% vs 3.24%)
- **Business conclusion:** Too small to matter

**However:** While the relationship is statistically significant, the effect size is minimal. Late deliveries reduce repeat rate by only 0.67 percentage points (2.57% vs 3.24%). Even if Olist eliminated all late deliveries, repeat rate would remain at 3.24% - still 97% churn.


## Test 2: Review Scores vs Repeat Purchases
**SQL Query - Extract Contingency Table:**
```sql
-- Chi-square test: Review score vs repeat behavior
SELECT 
    CASE 
        WHEN review_score >= 4 THEN 'Positive'
        WHEN review_score = 3 THEN 'Neutral'
        ELSE 'Negative'
    END as review_category,
    CASE WHEN is_repeat_customer = 1 THEN 'Repeat' ELSE 'OneTime' END as customer_type,
    COUNT(DISTINCT customer_unique_id) as count
FROM (
    SELECT DISTINCT ON (customer_unique_id)
        customer_unique_id,
        review_score,
        is_repeat_customer,
        order_purchase_timestamp
    FROM summary
    WHERE order_status = 'delivered' AND review_score IS NOT NULL
    ORDER BY customer_unique_id, order_purchase_timestamp
) first_orders
GROUP BY 
    CASE 
        WHEN review_score >= 4 THEN 'Positive'
        WHEN review_score = 3 THEN 'Neutral'
        ELSE 'Negative'
    END,
    CASE WHEN is_repeat_customer = 1 THEN 'Repeat' ELSE 'OneTime' END
ORDER BY review_category, customer_type;
```
**Query Results:**

<img width="311" height="183" alt="Screenshot 2025-11-30 at 5 09 28 PM" src="https://github.com/user-attachments/assets/870e847e-6401-4acd-9676-28b387a321a9" />



**Python - Chi-Square Test:**


```python
# Format will be:
# Negative-OneTime, Negative-Repeat, Neutral-OneTime, Neutral-Repeat, Positive-OneTime, Positive-Repeat
observed = [
    [11522, 368],  # Negative: OneTime, Repeat
    [7423, 244],   # Neutral: OneTime, Repeat
    [70840, 2325]  # Positive: OneTime, Repeat
]

chi2, p_value, dof, expected = chi2_contingency(observed)
print(f"Chi-square statistic: {chi2}")
print(f"P-value: {p_value}")
print(f"Conclusion: {'Significant' if p_value < 0.05 else 'Not significant'}")
```
**Test Results:**

<img width="416" height="130" alt="Screenshot 2025-11-30 at 5 10 35 PM" src="https://github.com/user-attachments/assets/0f85cece-b007-4480-af75-b973eca38edb" />

- **Null hypothesis:** First-order review score and repeat purchases are independent
- **Chi-square =** 0.23, p = 0.889 → Not statistically significant
- **Effect size:** ~0.08 percentage points (3.10% vs 3.18%)
- **Business conclusion:** Customer satisfaction doesn't affect retention at all

**Conclusion:** Statistical tests confirm that operational improvements (faster delivery, better reviews) will not solve the retention crisis. The problem is structural, not operational.          
Even when customers love their experience (5-star reviews, on-time delivery), they still don't come back.

