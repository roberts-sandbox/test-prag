---
layout: post
title: C# Closures
---

Consider the following code:

```csharp
public class Program
{
	private static Func<int>[] GetFunc()
	{
		int SIZE = 5;
		Func<int>[] func = new Func<int>[SIZE];
		for (int i = 0; i < SIZE; i++)
		{
			func[i] = () => i;
		}
		return func;
	}
	
	public static void Main(string[] args)
	{
		var funcs = Program.GetFunc();
		foreach (Func<int> func in funcs)
		{
			Console.Out.WriteLine(func());
		}          
	}
}
```

The method *GetFunc()* basically returns an array of *Func* that does not accept any argument but returns an int. When this program runs, the result is 5 lines of 5 that will be written to the console.  More than likely, this is not what one would intend.  The intend is probably 0 through 4 to be written to the console.    

The lambda in line 9

    func[i] = () => i;

could also be written as

    func[i] = delegate() { return i; };

Personally I prefer the lambda style over the anonymous delegate, but that's not the focal point.  Regardless of how it's written, it references the variable **i** (the loop index variable) which defined in its enclosing scope.  Notice that **i** is neither a local variable of the lambda nor an argument to it.  Because the lambda accesses the variable **i** that is defined in its enclosing scope, a closure is formed.  After the for loop from line 7 to line 10 executes, **i** will have the value of SIZE which is 5 (this is just basic *for* loop in general programming concept, nothing fancy here). When the method *GetFunc()* finishes its execution and goes out of scope, its returned value is the array of lambdas which are then assigned to the variable funcs in the *Main()* method.

Since the method *GetFunc()* already goes out of scope, its for loop index variable **i** stops to exist, right?  Yes, but only when closure is not involved.  As mentioned above, a closure is created because the array of lambdas returned "outlives" its enclosing scope *GetFunc()* and are still bound to **i** which is defined inside *GetFunc()*.  And remember that since the last value that **i** has is 5, when we invoke the lambda 5 times from line 17 to line 20, a value of 5 is written each time to the console.

Now consider this code:

```csharp
	class Program
	{
		class MyFunc
		{
			public Func<int> func { get; set; }
	   
			public MyFunc(int i)
			{
				func = () => i;
			}
		}

		private static MyFunc[] GetFunc()
		{
			int SIZE = 5;
			MyFunc[] myfuncs = new MyFunc[SIZE];

			for (int i = 0; i < SIZE; i++)
			{
				myfuncs[i] = new MyFunc(i);
			}

			return myfuncs;
		}

	
		public static void Main(string[] args)
		{
			var myfuncs = Program.GetFunc();            

			foreach (var myfunc in myfuncs)
			{
				Console.Out.WriteLine(myfunc.func());
			}

			Console.ReadLine();
		}
	}
```

There aren't that many differences between this and the first one.  But it works as you intended.  When you run the program, 0 through 4 are written to the console.

The difference between the first and second program is that in the first, the loop variable **i** is passed as parameter to the lambda function, whereas in the second, the loop variable **i** is passed to the constructor of *MyFunc* class which in turn is passed to the lambda.  It turns out that it is this difference that causes the 2 programs to behave differently.

In the second program, the lambda function does no longer bind to the loop index variable **i**.  Instead it binds to a copy of the parameter **i** that gets passed when the constructor is called.  As the loop index variable **i** keeps incrementing each time through the loop, this copy of **i** in the constructor always retains its original and only value.   

So this brings us back to how we can fix the fix the first program so it works as intended, i.e. like the second program but without having the define an extra class (*MyFunc*).  The answer is to define a temporary variable and and assign the loop index variable **i** to it.  It would look something like in the code below:

```csharp
class Program
{
    private static Func<int>[] GetFunc()
    {
        int SIZE = 5;
        Func<int>[] func = new Func<int>[SIZE];
        for (int i = 0; i < SIZE; i++)
        {
            int j = i;
            func[i] = () => j;
        }
        return func;
    }

    
    public static void Main(string[] args)
    {
        var funcs = Program.GetFunc();
        foreach (Func<int> func in funcs)
        {
            Console.Out.WriteLine(func());
        }

        Console.ReadLine();
    }
}
```

Each time through the loop, a separate, temporary variable **j** is created.  The lambda method instead of binding to the loop variable index **i**, it now binds to the variable **j**.  So even after the method *GetFunc()* finishes its execution, there are still 5 lambda functions that still have access to 5 separate, temporary variables **j** in their enclosing scope.  Since each copy of **j** has a different value (specifically 0 through 4), this allows the code to work correctly.  This is opposed to the first program, which the 5 lambda functions still access a single loop index variable **i** after *GetFunc()* finishes its execution (at which point the loop index variable is 5, hence that explains why 5 lines of 5 are written to the console).

I'd like to thank [Tim Rayburn](http://timrayburn.net/) for "inspiring" me to write this article.