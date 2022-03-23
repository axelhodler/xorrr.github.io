---
layout: post
title: "Implementing Binary Search with TDD"
date:   2021-07-06 9:22:13
description: ""
tags: []
---

Some people might be able to implement the binary search algorithm by heart. Correctness could, if at all, be proven by feeding the algorithm a few combinations of a number and an ordered list of numbers.

Instead of keeping the algorithm in the language of your choice, or pseudo code, in your head it is easier to know the gist of what it does and then to arrive at the implementation step by step using Test Driven Development. As a side effect we both specify how it works, document the edge cases and make sure it works as intended.

The post is serves as an introduction to create the algorithm using Kotlin as the programming language and JUnit 5 as the testing framework.

We start with a simple test to create the structure of our method and describing what it does

{% highlight kotlin %}
@Test
fun `finds index (location) of a number in an ordered list`() {
  val orderedList = listOf(1)
  val index = binarySearch(1, orderedList)
  assertThat(index).isEqualTo(0)
}
{% endhighlight %}

The code won't compile yet. There is no `binarySearch` function. We implement the function.

{% highlight kotlin %}
fun binarySearch(number: Int, orderedListToSearch: List<Int>): Int {
  return 123
}
{% endhighlight %}

The parameters are the `number` where we want to find the index for and the `orderedListToSearch`.
Running the test again will fail. Finding the index of `1` in `listOf(1)` is not `123`.
We attempt to do the least amount of required work to get to a passing test by changing the implementation of `binarySearch`

{% highlight kotlin %}
fun binarySearch(number: Int, orderedListToSearch: List<Int>): Int {
  return 0
}
{% endhighlight %}

## Handle the not found case!

We could return `null`. Or we return `-1` to avoid changing the function. One might state using the magic number `-1` for the concept not found is bad. So is using `null` for the concept not found. Let's use `null` just it is more common in the world of Java.

The test passes. Let's create a second test where the expected index is not `0`.

{% highlight kotlin %}
@Test
fun `returns index 1 for number 2 in (1, 2, 3)`() {
    val orderedList = listOf(1, 2, 3)
    val index = binarySearch(2, orderedList)
    assertThat(index).isEqualTo(1)
}
{% endhighlight %}

The test passes, the `index` is `1`, not `0`.

We adapt the implementation

{% highlight kotlin %}
fun binarySearch(number: Int, listToSearch: List<Int>): Int {
  return if (number == 2) {
    1
  } else {
    0
  }
}
{% endhighlight %}

The test passes. We already know as soon as we add another test with a different number the implementation will fail again. Let's do the refactoring step.
Can we remove the `if` `else` branching? Yes.

{% highlight kotlin %}
fun binarySearch(number: Int, listToSearch: List<Int>): Int {
  return listToSearch.binarySearch(number)
}
{% endhighlight %}

The Kotlin `List` already ships with a `binarySearch` function. So should we stop here? Most programming languages ship with a `binarySearch` function. Thus most programmers will never have to implement Binary Search by themselves. Still, lets continue and try to build our own binary search.

{% highlight kotlin %}
fun binarySearch(number: Int, listToSearch: List<Int>): Int {
  return if (number == 2) {
    1
  } else {
    0
  }
}
{% endhighlight %}

We create a third test.

{% highlight kotlin %}
@Test
fun `returns index 2 for number 3 in (1, 2, 3)`() {
    val orderedList = listOf(1, 2, 3)
    val index = binarySearch(3, orderedList)
    assertThat(index).isEqualTo(2)
}
{% endhighlight %}

If we go for a simple solution which passes the tests we can search the list element by element.


