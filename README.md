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
```
[]
[(x:?)]
[(z:?), (x:?)]
[(z:4), (x:?)]
[f{}, (z:4), (x:?)]
[(x':3), f{}, (z:4), (x:?)]
[(x'':2), (x':3), f{}, (z:4), (x:?)]
[(x''':1), (x'':2), (x':3), f{}, (z:4), (x:?)]
[(x'''':0), (x''':1), (x'':2), (x':3), f{}, (z:4), (x:?)]
[(x’’’:1), (x’’:2), (x’:3), f{}, (z:1), (x:?)]
[(x’’:2), (x’:3), f{}, (z:3), (x:?)]
[(x’:3), f{}, (z:11), (x:?)]
[f{}, (z:123), (x:123)]

After line 12 executes, the value of z is:
- z = 123
```

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
```
[]
[(x:?)]
[(y:?), (x:?)]
[(z:?), (y:?), (x:?)]
[(z:?), (y:?), (x:4)]
[(z:?), (y:6), (x:4)]
[f{}, (z:?), (y:6), (x:4)]
[(y':?), f{}, (z:?), (y:6), (x:4)]
[(y':13), f{}, (z:?), (y:6), (x:4)]
[g{}, (y':13), f{}, (z:?), (y:6), (x:4)]
[(y'':?), g{}, (y':13), f{}, (z:?), (y:6), (x:4)]
[(y'':14), g{}, (y':13), f{}, (z:?), (y:6), (x:4)]
[g{}, (y':13), f{}, (z:52), (y:6), (x:4)]

After line 12 executes, the value of z is:
- z = 52
```

2. Draw the runtime stack after each line executes under **dynamic scoping**. What value assigned to z in line 12?
```
[]
[(x:?)]
[(y:?), (x:?)]
[(z:?), (y:?), (x:?)]
[(z:?), (y:?), (x:4)]
[(z:?), (y:6), (x:4)]
[f{}, (z:?), (y:6), (x:4)]
[(y':?), f{}, (z:?), (y:6), (x:4)]
[(y':13), f{}, (z:?), (y:6), (x:4)]
[g{}, (y':13), f{}, (z:?), (y:6), (x:4)]
[(y'':?), g{}, (y':13), f{}, (z:?), (y:6), (x:4)]
[(y'':14), g{}, (y':13), f{}, (z:?), (y:6), (x:4)]
[g{}, (y':13), f{}, (z:42), (y:6), (x:4)]

After line 12 executes, the value of z is:
- z = 42
```

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
```
[]
[(y:?)]
[(z:?), (y:6)]
[f{}, (z:?), (y:6)]
[g{}, f{}, (z:?), (y:6)]
[x:y*2, g{}, f{}, (z:?), (y:6)]   | x = 12
[a:x+1, x:y*2, g{}, f{}, (z:?), (y:6)]   | a = 12+1 = 13
[a:x+1, x:y*2, g{}, f{}, (z:?), (y:14)]   | y = 13+1 = 14; a = 14+13 = 27
[res:27, a:x+1, x:y*2, g{}, f{}, (z:?), (y:14)]
[x:27, g{}, f{}, (z:?), (y:45)]   | y = 27+2+16 = 45
[x:27, g{}, f{}, (z:?), (y:48)]
[a:x-y+2, x:27, g{}, f{}, (z:?), (y:48)]   | a = 27-48+2 = -19
[a:x-y+2, x:27, g{}, f{}, (z:98), (y:48)] | y = -19+1 = -18; a = 48-18 = 30
[res:98, a:x-y+2, x:27, g{}, f{}, (z:98), (y:48)]
[x:27, g{}, f{}, (z:98), (y:48)]
[x:27, g{}, f{}, (z:99), (y:48)]
[g{}, f{}, (z:99), (y:48)]

After line 17 executes, the values of y and z are:
- y = 48
- z = 99
```

3. Draw the runtime stack after each line executes given that both parameters a and x are passed using **Call-by-Need**. What are the values of y and z after line 13 executes?
```
[]
[(y:?)]
[(z:?), (y:6)]
[f{}, g{}, (z:?), (y:6)]
[x:y*2, f{}, g{}, (z:?), (y:6)] | x = 6*2 = 12
[a:x+1, x:y*2, f{}, g{}, (z:?), (y:6)] | a = 12+1 = 13
[a:x+1, x:y*2, f{}, g{}, (z:?), (y:14)] | y = 13+1 = 14
[res:27, a:x+1, x:y*2, f{}, g{}, (z:?), (y:14)]
[x:27, f{}, g{}, (z:?), (y:29)] | y = 27+2 = 29
[a:x-y+2, x:27, f{}, g{}, (z:?), (y:29)] | a = 27-29+2 = 0
[a:x-y+2, x:27, f{}, g{}, (z:?), (y:1)] | y = 0+1 = 1; a = 0
[res:1, a:x-y+2, x:27, f{}, g{}, (z:?), (y:1)]
[x:27, f{}, g{}, (z:-28), (y:-14)] | z = 1-1-28 = -28; y = 1-15 = -14
[f{}, g{}, (z:-28), (y:-14)]

After line 17 executes, the values of y and z are:
- y = -14
- z = -28
```
