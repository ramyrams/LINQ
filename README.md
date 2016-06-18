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
```cs
int[] numbers = { 2, 5, 28, 31, 17, 16, 42 };

// Query syntax
var numsQuery = from n in numbers 
                where n < 20
                select n;
// Method syntax
var numsMethod = numbers.Where(x => x < 20);

// Combined
int numsCount = (from n in numbers 
                    where n < 20
                    select n).Count();

foreach (var x in numsQuery)
    Console.Write("{0}, ", x);  //output: 2, 5, 17, 16,

foreach (var x in numsMethod)
    Console.Write("{0}, ", x);  //output: 2, 5, 17, 16,

Console.WriteLine(numsCount);   //output: 4

```

## Query Variables


int[] numbers = { 2, 5, 28 };
IEnumerable<int> lowNums = from n in numbers // Returns an enumerator
                            where n < 20
                            select n;
                            
int numsCount = (from n in numbers // Returns an int
                    where n < 20
                    select n).Count();


## From Clause
```cs

var groupA = new[] { 3, 4, 5, 6 };
var groupB = new[] { 6, 7, 8, 9 };

var someInts =  from a in groupA     // Required first from clause
                from b in groupB    // First clause of query body
                where a > 4 && b <= 8
                select new {a, b, sum = a + b}; //Object of anonymous type

foreach (var a in someInts)
    Console.WriteLine(a);

//Output        
{ a = 5, b = 6, sum = 11 }
{ a = 5, b = 7, sum = 12 }
{ a = 5, b = 8, sum = 13 }
{ a = 6, b = 6, sum = 12 }
{ a = 6, b = 7, sum = 13 }
{ a = 6, b = 8, sum = 14 }

```

## Let Clause
```cs
var groupA = new[] { 3, 4, 5, 6 };
var groupB = new[] { 6, 7, 8, 9 };

var someInts =  from a in groupA
                from b in groupB
                let sum = a + b //Store result in new variable.
                where sum == 12
                select new {a, b, sum};

foreach (var a in someInts)
    Console.WriteLine(a);

//Output
{ a = 3, b = 9, sum = 12 }
{ a = 4, b = 8, sum = 12 }
{ a = 5, b = 7, sum = 12 }
{ a = 6, b = 6, sum = 12 }
```


## Join Clause
```cs
var query = from s in students
            join c in studentsInCourses on s.StID equals c.StID
            where c.CourseName == "History"
            select s.LastName;
```




## Join
```cs

```


