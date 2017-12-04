---
layout: post
title: "A sliding Knight Jump"
date: 2017-12-03 12:00:00
categories: [implementation, c++]
featured_image: /images/cover.jpg
---

This is a small tricks I came up with when solving [ UVA 11906][1].

I suspect it works on any polyhedron coordinates centering
at the origin, but haven't tested it yet.

## The problem
So in UVA 11906, the first step of solving this problem is moving around
the grid in a Knight's pattern, but instead of the 2 by 1, they generalized
it to a ***m*** by ***n*** movement.

![All Knights moves]({{ "assets/knight.jpg" | relative_url }})

## Code

Since there are 8 possible after position for this knight,
I don't want to go ahead and write this ugly piece of code.
For simplicity, we assume that m = 2 and n = 1.

{% highlight c %}
depthFirstSearch(x+2, y+1);
depthFirstSearch(x+1, y+2);

depthFirstSearch(x-2, y+1);
depthFirstSearch(x-1, y+2);

depthFirstSearch(x-2, y-1);
depthFirstSearch(x-1, y-2);

depthFirstSearch(x+2, y-1);
depthFirstSearch(x+1, y-2);


{% endhighlight %}



Ughh, even as I was typing down this post,
You have to triple check to make sure that you have covered all the coordinates,
and that you did not duplicate any node. So I started drawing pictures on paper
and noticed something funny

![All Knights moves]({{ "assets/knightcoordinate.png" | relative_url }})

## Better code


If you chain point {n, m} and {m, -n} together, you could get a triplet {n, m, -n}
which contains both {m, n} and {n, m} when using a sliding window of size 2 to progress through the array.

This is something from my networking class, I remember vividly that you can encode your message in such a way
that, when use with sliding window, list all posible permutation of your code. If you know what algorithm it is please
***DO*** tell me.

With that in mind, let's group all the points into such sequence, this is very much like playing domino.

I pick the next point in the chain by intuition, but to be more explicit the trick is
to replace x coordinate with previous y coordinate, and y coordinate with the minus of previous x coordinate

{% highlight c %}
{ n, m}
{ m, -n}
{ -n, -m}
{ -m, n}
{ n, -m}
{ -m, -n}
{ -n, m}
{ m, n}
{% endhighlight %}

The code belows print all possible jumps of a knight

{% highlight c %}
int moves[9] = {n, m, -n, -m, n, -m, -n, m, n};
void knightJump(int x, int y){
  for(int i = 0; i < 8; ++i){
    printf("(%d, %d )\n", x+ moves[i], y+moves[i+1]);
  }
}

{% endhighlight %}

[1]: <https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=16&page=show_problem&problem=3057>

