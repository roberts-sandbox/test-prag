---
layout: post
title: Hello World
published: true
---

This is my first post.  This is an example code:

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

That's it.