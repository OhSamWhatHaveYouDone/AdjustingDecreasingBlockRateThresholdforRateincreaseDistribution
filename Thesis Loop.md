```python
txtfile=open('consumerduke.txt')

consumer_usage=[]
for line in txtfile:
    consumer_usage.append(float(line.rstrip()))
txtfile.close()

def calculate_revenue(tier_limits, tier_prices, consumer_usage):
    revenue = 0
    for usage in consumer_usage:
        for i, limit in enumerate(tier_limits):
            if usage <= limit:
                revenue += usage * tier_prices[i]
                break
            else:
                revenue += limit * tier_prices[i]
                usage -= limit
    return revenue

def adjust_price_structure(consumer_usage, target_increase):
    # Initial price structure
    current_tier_limits = [300, 700, float('inf')]
    current_tier_prices = [0.14879, 0.108279, 0.098147]

    # Calculate the current revenue
    current_revenue = calculate_revenue(current_tier_limits, current_tier_prices, consumer_usage)
    prev_revenue = current_revenue
    # Perform iterative adjustments to reach the target increase
    while target_increase - current_revenue > 0.99:
        # Increase the quantity limit of the first tier by 1 kWh
        current_tier_limits[0] += 1

        # Recalculate the revenue with the updated price structure
        current_revenue = calculate_revenue(current_tier_limits, current_tier_prices, consumer_usage)
        if current_revenue == prev_revenue:
            print("Nah")
            print(str(current_revenue) + " is current revenue")
            break
        prev_revenue = current_revenue
        
    return current_tier_limits, current_tier_prices

# Example data 
target_increase =    110042.92 

# Get the adjusted price structure
adjusted_tier_limits, adjusted_tier_prices = adjust_price_structure(consumer_usage, target_increase)

# Output the result
print("Adjusted Price Structure:")
for i in range(len(adjusted_tier_limits)):
    print(f"Up to {adjusted_tier_limits[i]} kWh: ${adjusted_tier_prices[i]:.5f} per kWh")
```

    Nah
    95696.58813342714 is current revenue
    Adjusted Price Structure:
    Up to 1174 kWh: $0.14879 per kWh
    Up to 700 kWh: $0.10828 per kWh
    Up to inf kWh: $0.09815 per kWh
    


```python

```
