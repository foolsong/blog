###high cyclomatic complexity

>Cyclomatic complexity is determined by the number of linearly independent paths through a program’s source code. In other words, cyclomatic complexity of a method is measured by the number of decision points, like if, while, and for statements, plus one for the method entry.

>The experiments McCabe, the author of cyclomatic complexity, conclude that methods in the 3 to 7 complexity range are quite well structured. He also suggest the cyclomatic complexity of 10 is a reasonable upper limit.

```
	void example(int a, int b, int c) // 1
	{
	    if (a == b)                   // 2
	    {
	        if (b == c)               // 3
	        {
	        }
	        else if (a == c)          // 3
	        {
	        }
	        else
	        {
	        }
	    }
	    for (int i = 0; i < c; i++)   // 4
	    {
	    }
	    switch(c)
	    {
	        case 1:                   // 5
	            break;
	        case 2:                   // 6
	            break;
	        default:                  // 7
	            break;
	    }
	}
```

>Thresholds:
CYCLOMATIC_COMPLEXITY The cyclomatic complexity reporting threshold, default value is 10. 

>Suppress:
`__attribute__((annotate("oclint:suppress[high cyclomatic complexity]")))`






###high ncss method

>This rule counts number of lines for a method by counting Non Commenting Source Statements (NCSS). NCSS only takes actual statements into consideration, in other words, ignores empty statements, empty blocks, closing brackets or semicolons after closing brackets. Meanwhile, a statement that is broken into multiple lines contribute only one count.


```
	void example()          // 1
	{
	    if (1)              // 2
	    {
	    }
	    else                // 3
	    {
	    }
	}
```

>Thresholds:
NCSS_METHOD The high NCSS method reporting threshold, default value is 30. 

>Suppress:
`__attribute__((annotate("oclint:suppress[high ncss method]")))`


###high npath complexity

>NPath complexity is determined by the number of execution paths through that method. Compared to cyclomatic complexity, NPath complexity has two outstanding characteristics: first, it distinguishes between different kinds of control flow structures; second, it takes the various type of acyclic paths in a flow graph into consideration.

>Based on studies done by the original author in AT&T Bell Lab, an NPath threshold value of 200 has been established for a method.


```
	void example()
	{
	    // complicated code that is hard to understand
	}
```

>Thresholds:
NPATH_COMPLEXITY The NPath complexity reporting threshold, default value is 200. 

>Suppress:
__attribute__((annotate("oclint:suppress[high npath complexity]"))) 

>References:
Brian A. Nejmeh (1988). [“NPATH: a measure of execution path complexity and its applications”](http://dl.acm.org/citation.cfm?id=42379). Communications of the ACM 31 (2) p. 188-200


