# LINQ


```cs
// Data source
int[] numbers = { 2, 12, 5, 15 };

// Define and store the query.
IEnumerable<int> lowNums = from n in numbers
                            where n < 10
                            select n;
// Execute the query.
foreach (var x in lowNums) 
  Console.Write("{0}, ", x);

```


## Anonymous Type
```cs

//Anonymous Types1
var student = new { Name = "Mary Jones", Age = 19, Major = "History" };
Console.WriteLine("{0}, Age {1}, Major: {2}", student.Name, student.Age, student.Major);

//Anonymous Types2
string Major = "History";
var student = new { Age = 19, Other.Name, Major };

//Anonymous Types3
var student = new { Age = Age, Name = Other.Name, Major = Major };
```



## Method Syntax and Query Syntax
