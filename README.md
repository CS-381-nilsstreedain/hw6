# Homework 6 – Scope and Parameters
## Problem 1. Runtime Stack
Consider the following block. Assume static scoping and call-by-value parameter passing.
```c
{	int x;
	int z;
	Z:=4;
	{	int f(int x) {
		if x==0 then {
			z := 1	}
		else {
			z := f(x-1)*z+2	};
		return z;
		};
		x := f(3);
	};
};
```
Demonstrate the computations that take place during the evaluation of this block, that is, give a sequence of lines each showing the complete runtime stack with all activation records after each statement or function call. For recursive calls use one stack onto which a new activation record is pushed on for each recursive function call.

## Problem 2. Static and Dynamic Scope
Consider the following block. Assume call-by-value parameter passing.
```c
{	int x;
	int y;
	int z;
	x := 4;
	y := 6;
	{	int f(int y) {	return x*y };
		int y;
		y := 13;
		{	int g(int x) {	return f(y)	};
			{	int y;
				y := 14;
				z := g(3);
			};
		};
	};
};
```
1. Draw the runtime stack after each line executes under **static scoping**. What value assigned to z in line 12?
2. Draw the runtime stack after each line executes under **dynamic scoping**. What value assigned to z in line 12?

## Problem 3. Parameter Passing
Consider the following block. Assume dynamic scoping.
```c
{	int y;
	int z;
	y := 6;
	{	int f(int a) {
			y := a+1;
			return (y+a)
		};
		int g(int x) {
			y := f(x+1)+2;
			z := f(x-y+2);
			return (z+1)
		}
		z := g(y*2);
	};
};
```
1. Draw the runtime stack after each line executes given that both parameters a and x are passed using **Call-by-Name**. What are the values of y and z after line 13 executes?
2. Draw the runtime stack after each line executes given that both parameters a and x are passed using **Call-by-Need**. What are the values of y and z after line 13 executes?
