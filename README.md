#Recursive algorithms:

Here are some recursive algorithm challenges culled from Wikipedia, HackerRank, Project Euler, and elsewhere.

If your code is inefficient, consider looking into [memoization](https://en.wikipedia.org/wiki/Memoization) and [dynamic programming](https://en.wikipedia.org/wiki/Dynamic_programming).

##Greatest Common Divisor:

Implement Euclid's algorithm to find the greatest common denominator of two positive integers.  
	`gcd(a,a) = a`
	`gcd(a,b) = gcd(a-b,b)` if a > b
	`gcd(a,b) = gcd(a, b-1)` if b > a
	
Now find `gcd(1529, 14038)`, `gcd(1529, 14039)`, and `gcd(123456, 987654)`

##Ackermann Function

The Ackermann function has an important place in the history of computer theory; it was the first example of a computable function that is not primitive recursive.  (I *think* that means that it can't be calculated with just for-loops, as those run for a set number of steps, as opposed to the potentially-unbound while-loop.  You can try to get more details from the [Wikipedia page](https://en.wikipedia.org/wiki/Primitive_recursive_function).)  Here's the definition:
	`A(x,y) = y + 1` if x = 0
	`A(x,y) = A(x-1,1)` if y = 0 
	`A(x,y) = A(x-1,A(x,y-1))` otherwise

Calculate `A(3,2)` and `A(3,6)`.  If you're feeling bold, try `A(6,3)`.

##Zibonacci

Consider the function z(n):
	`z(0) = 1`
	`z(1) = 1`
	`z(2) = 2`
	`z(2*n) = z(n+1) + z(n) + 1` if n >= 2
	`z(2*n + 1) = z(n-1) + z(n) + 1` if n >= 1

What is `z(10)`?  `z(100)`?

[I was given a harder version as part of Google Foobar: For a given X, find the largest n such that z(n) = X.  I don't think a recursive solution is the best fit for this, but then again, I never found any good answer.]

##Tak Function

The tak function is used to benchmark how well platforms handle recursion.  Unlike some other functions here, the numbers don't get particularly large, but there are very many function calls.
	`tak(x,y,z) = y` if x <= y
	`tak(x,y,z) = tak(tak(x-1, y, z), tak(y-1,z,x), tak(z-1,x,y))` otherwise

Find `tak(6,3,5)` and `tak(10,2,9)`.  In both cases, how many times is tak called?

##Collatz

Consider the function f(n):
	`f(n) = n/2` if n is even
	`f(n) = 3*n + 1` if n is odd

Start with any positive integer, then take `f(n)`, `f(f(n))`, `f(f(f(n)))`, and so forth.  The Collatz conjecture states that this sequence will eventually reach the cycle 4, 2, 1, 4, 2, 1... regardless of the starting n.

What starting number under 10,000 has the most terms before reaching 1 for the first time?

##Change-making

Assuming you can only use quarters, dimes, nickels, and pennies, return every way to make 37 cents, and every way to make a dollar.

##Balanced Parentheses

This was done in class, but it's worth doing again.  Write a function that takes a parameter n and returns every possible balanced combination of n opening and closing parentheses.  For example, for input 3, it should return `["((()))", "()(())", "()()()", "(())()", "(()())"]` (possibly in a different order, of course).

As a double check, the total number of strings returned should be equal to (2n)!/(n!*(n+1)!).

##Shortest Path

Some background on graph theory: A graph has some number of nodes.  Some or all nodes have edges between them.  Each edge has a weight; most often we're looking to find a path that has the lowest weight, as we are in this case.

![weighted graph](http://web.cs.wpi.edu/~mebalazs/cs507/slides06/slides-4.gif)

The picture makes it easy to see the structure of the graph, but it's not very machine readable.  A more useful format is an adjaceny matrix, a 2d array showing the distance between each pair of connected nodes.  For our purposes, I'm going to write it as a dictionary of dictionaries:

```
{
	"A": {"C": 5, "D": 2},
	"B": {"D": 4, "F": 8},
	"C": {"A": 5, "G": 4},
	"D": {"A": 2, "B": 4, "E": 3, "H": 7},
	"E": {"D": 3, "G": 5, "H": 2},
	"F": {"B": 8, "H": 6},
	"G": {"C": 4, "E": 5},
	"H": {"D": 7, "E": 2, "F": 6},
}
```

Dijkstra's algorithm is used to find the shortest distance between two points (referred to here as `shortest(x,y)`) by considering the shortest path between each of the starting cell's neighbors and the destination cell, then the starting cell's neighbors' neighbors, and so forth.  For example, `shortest(A,G)` is equal to `min(shortest(D,G)+2, shortest(C,G)+5)`, which is equal to `min(shortest(B,G)+6, shortest(E,G)+5, shortest(H,G)+9, 9)`, and so forth.  (Confused about where that 9 at the end came from?  There's only one way to get from C to G without backtracking, so we know that `shortest(C,G)` must equal 9.)

Calculate `shortest(C,F)`.

Now consider the graph represented by this (randomly-generated) adjacency matrix:

```
{	
	"A": {'B': 8, 'E': 6},
	"B": {'A': 8, 'C': 8, 'E': 1},
	"C": {'B': 8, 'D': 8, 'F': 5},
	"D": {'C': 8, 'E': 1, 'F': 2, 'G': 10, 'H': 5},
	"E": {'A': 6, 'B': 1, 'D': 1, 'F': 3},
	"F": {'C': 5, 'D': 2, 'E': 3, 'G': 8},
	"G": {'D': 10, 'F': 8, 'H': 7, 'I': 7, 'K': 2},
	"H": {'D': 5, 'G': 7, 'I': 6, 'J': 7},
	"I": {'G': 7, 'H': 6, 'J': 8, 'L': 2},
	"J": {'H': 7, 'I': 8, 'K': 8, 'L': 4},
	"K": {'G': 2, 'J': 8, 'L': 8},
	"L": {'I': 2, 'J': 4, 'K': 8},
}
```

Calculated `shortest(A,L)`.