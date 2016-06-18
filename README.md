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

// Array of objects of an anonymous type
var students = new[] 
{
    new { LName="Jones", FName="Mary", Age=19, Major="History" },
    new { LName="Smith", FName="Bob", Age=20, Major="CompSci" },
    new { LName="Fleming", FName="Carol", Age=21, Major="History" }
};

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







## Where Clause
```cs
var groupA = new[] { 3, 4, 5, 6 };
var groupB = new[] { 6, 7, 8, 9 };

var someInts =  from int a in groupA
                from int b in groupB
                let sum = a + b
                where sum >= 11 // Condition 1
                where a == 4    // Condition 2
                select new {a, b, sum};

foreach (var a in someInts)
    Console.WriteLine(a);

//Output
{ a = 4, b = 7, sum = 11 }
{ a = 4, b = 8, sum = 12 }
{ a = 4, b = 9, sum = 13 }
```






## orderby Clause
```cs
var query = from student in students
            orderby student.Age ← Order by Age.
            select student;
```






## group Clause
```cs
var students = new[] // Array of objects of an anonymous type
{
    new { LName="Jones", FName="Mary", Age=19, Major="History" },
    new { LName="Smith", FName="Bob", Age=20, Major="CompSci" },
    new { LName="Fleming", FName="Carol", Age=21, Major="History" }
};

var query = from student in students
            group student by student.Major;
foreach (var s in query) // Enumerate the groups.
{
    Console.WriteLine("{0}", s.Key);  //Grouping key

    foreach (var t in s) // Enumerate the items in the group.
    Console.WriteLine(" {0}, {1}", t.LName, t.FName);
}

//Output
History
    Jones, Mary
    Fleming, Carol
CompSci
    Smith, Bob

```




## into Clause
```cs
var groupA = new[] { 3, 4, 5, 6 };
var groupB = new[] { 4, 5, 6, 7 };

var someInts =  from a in groupA
                join b in groupB on a equals b
                into groupAandB // Query continuation
                from c in groupAandB
                select c;

foreach (var a in someInts)
    Console.Write("{0} ", a);
```




## Standard Query Operators
```cs

int[] numbers = new int[] {2, 4, 6};
int total = numbers.Sum();
int howMany = numbers.Count();


int[] intArray = new int[] { 3, 4, 5, 6, 7, 9 };

// Method syntax
var count1 = Enumerable.Count(intArray); 
var firstNum1 = Enumerable.First(intArray); 

// Extension syntax
var count2 = intArray.Count(); 
var firstNum2 = intArray.First(); 

Console.WriteLine("Count: {0}, FirstNumber: {1}", count1, firstNum1);
Console.WriteLine("Count: {0}, FirstNumber: {1}", count2, firstNum2);

//Output
Count: 6, FirstNumber: 3
Count: 6, FirstNumber: 3

```


## Query Expressions
```cs
var numbers = new int[] { 2, 6, 4, 8, 10 };
int howMany = (from n in numbers
                where n < 7
            select n).Count();  // Query expression and Operator

Console.WriteLine("Count: {0}", howMany);

//Output 
Count: 3
```



## Delegates As Parameters
```cs
int[] intArray = new int[] { 3, 4, 5, 6, 7, 9 };
var countOdd = intArray.Count(n => n % 2 == 1); //Lambda expression identifying the odd values
Console.WriteLine("Count of odd numbers: {0}", countOdd);

//Output
Count of odd numbers: 4
```


## Lambda Expression Parameter
```cs
int[] intArray = new int[] { 3, 4, 5, 6, 7, 9 };

Func<int, bool> myDel = delegate(int x)     //Anonymous method
                        {
                            return x % 2 == 1;
                        };
var countOdd = intArray.Count(myDel);

Console.WriteLine("Count of odd numbers: {0}", countOdd);
```


## Lambda Expression Parameter
```cs
```

