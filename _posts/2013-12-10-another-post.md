---
layout: post
title: Another Post
published: true
---

Another post to say *Hello World again*.  The Markdown reference is at [Markdown Reference](http://daringfireball.net/projects/markdown/).


The following is an example of Github flavored Markdown code:

``` csharp
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
