---
layout: post
title: "Understanding the knapsack problem"
date: 2024-01-29
# categories: algorithms
---

![knapsack header image](/assets/knapsack-header.png)

I recently solved a leet code (maximum profit trading stocks) challenge that introduced me to a dynamic programming problem called knapsack.

# The knapsack problem

The knapsack problem is an optimization challenge characterized by a knapsack with a limited capacity. In this scenario, you possess a set of items, each with a specific weight and value. The objective is to maximize the total value of the items you can accommodate in the knapsack without surpassing its weight capacity. The task involves determining the optimal combination of items to pack into the knapsack, aiming to achieve the highest overall value within the given weight limit.

We have 3 main variables:

1. Weight
2. Value
3. Capacity

Well give these values to make this more concrete

```jsx
Weight = [5, 4, 6, 2, 3] 
Values = [2, 1, 3, 4, 5]
Capacity = 10
```

We’ll go ahead and create an array of the length of ‘capacity’ + 1. Each index in the array represents capacity.

![23](/assets/knapsack-array1.jpg)

We will loop through each item and fill each index with the maximum value for the capacity so far.
The first item has a value of 2 and a weight of 5.

![23](/assets/knapsack-weight5.jpg)

As you can see, for the first item with a weight of 5 we can only hold 2 when the minimum capacity is 5. Let’s go to the next item.

Since we are aiming to find the maximum value of all the items considered thus far within the given capacity, for the next item, we must compare two values. We compare the current value, which is 2, and, considering the item's weight is 4, we also assess if we could accommodate the maximum value of 6, given the capacity is 10. Which we can so we add those values together in the at index 10 (capacity of 10). Subsequently, at a capacity of 9, we evaluate the maximum value at 5, and so on.

![knapsack item-2](/assets/knapsack-weight4.jpg)

The code so far would look something like this:

```jsx
function knapsackBottomUp(weights, values, capacity){
  let dp = new Array(capacity + 1).fill(0);
  for (let i = 0; i < weights.length; i++) {
    let value = values[i];
    for (let j = capacity; j >= weights[i]; j--) {
      dp[j] = Math.max(dp[j], dp[j - weights[i]] + value);
    }
  }
  ...
}
```

We will continue filling this out. As you can see, we are breaking down our problem into subproblems and populating the maximum values for each capacity limit. After iterating through each item, the resulting dp array would look something like this:

![knapsack all weights](/assets/knapsack-weightsall.jpg)

From here we can just return the last index after we are finished calculating the array with the last item. Voila! The maximum value at a capacity of 10 is 11.

Here is the full code in JavaScript:

```jsx

function knapsackBottomUp(weights, values, capacity){
  let dp = new Array(capacity + 1).fill(0);
  for (let i = 0; i < weights.length; i++){
    let value = values[i];
    for (let j = capacity; j >= weights[i]; j--) {
      dp[j] = Math.max(dp[j], dp[j - weights[i]] + value);
    }
  }
  return dp[capacity];
}

```
Please send me an email, if you see any issues or have any suggestions!<br>
<b>tristansetha@gmail.com</b>

<!-- 
`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk]. -->

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]: https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
