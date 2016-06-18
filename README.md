# LINQ


## Lambda Expressions
```cs
( ArgumentsToProcess ) => { StatementsToProcessThem }

// C# lambda expression.
List<int> evenNumbers = list.FindAll(i => (i % 2) == 0);

```


## Extension Methods
```cs
namespace ExtensionMethods
{
    public static class MyExtensions
    {
        public static int WordCount(this String str)
        {
            return str.Split(new char[] { ' ', '.', '?' }, 
                             StringSplitOptions.RemoveEmptyEntries).Length;
        }
    }   
}


using ExtensionMethods;
string s = "Hello Extension Methods";
int i = s.WordCount();


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



## Basic
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
  
  
//Projecting New Data Types
var nameDesc = from p in products select new { p.Name, p.Description };  

return nameDesc; // Nope!   //static var GetProjectedSubset(ProductInfo[] products)

// Map set of anonymous objects to an Array object.
return nameDesc.ToArray();  //static Array GetProjectedSubset(ProductInfo[] products)


```



##LINQ As a Better Venn Diagramming Tool
```cs

List<string> myCars = new List<String> {"Yugo", "Aztec", "BMW"};
List<string> yourCars = new List<String>{"BMW", "Saab", "Aztec" };


var carDiff =(from c in myCars select c)
	.Except(from c2 in yourCars select c2);


Console.WriteLine("Here is what you don't have, but I do:");

foreach (string s in carDiff)
	Console.WriteLine(s); // Prints Yugo.


// Get the common members.
var carIntersect = (from c in myCars select c)
.Intersect(from c2 in yourCars select c2);

Console.WriteLine("Here is what we have in common:");
foreach (string s in carIntersect)
Console.WriteLine(s); // Prints Aztec and BMW.




// Get the union of these containers.
var carUnion = (from c in myCars select c)
.Union(from c2 in yourCars select c2);
Console.WriteLine("Here is everything:");
foreach (string s in carUnion)
Console.WriteLine(s); // Prints all common members.




var carConcat = (from c in myCars select c)
.Concat(from c2 in yourCars select c2);
// Prints: Yugo Aztec BMW BMW Saab Aztec.
foreach (string s in carConcat)
Console.WriteLine(s);

```


## LINQ Aggregation Operations
```cs
double[] winterTemps = { 2.0, -21.3, 8, -4, 0, 8.2 };

// Various aggregation examples.
Console.WriteLine("Max temp: {0}",
(from t in winterTemps select t).Max());

Console.WriteLine("Min temp: {0}",
(from t in winterTemps select t).Min());

Console.WriteLine("Avarage temp: {0}",
(from t in winterTemps select t).Average());

Console.WriteLine("Sum of all temps: {0}",
(from t in winterTemps select t).Sum());


```



## With & Without LINQ
```cs
// Assume we have an array of strings.
string[] currentVideoGames = { "Morrowind", "Uncharted 2", "Fallout 3", "Daxter", "System Shock 2" };
string[] gamesWithSpaces = new string[5];

for (int i = 0; i < currentVideoGames.Length; i++)
{
    if (currentVideoGames[i].Contains(" "))
        gamesWithSpaces[i] = currentVideoGames[i];
}

// Now sort them.
Array.Sort(gamesWithSpaces);

// Print out the results.
foreach (string s in gamesWithSpaces)
{
    if (s != null)
        Console.WriteLine("Item: {0}", s);
}

//With Linq
// Build a query expression to find the items in the array that have an embedded space.
IEnumerable<string> subset = from g in currentVideoGames
                            where g.Contains(" ") orderby g select g;
                            
// Print out the results.
foreach (string s in subset)
Console.WriteLine("Item: {0}", s);


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



// Find the fast BMWs!
var fastCars = from c in myCars where c.Speed > 90 && c.Make == "BMW" select c;


```


## Applying LINQ Queries to Nongeneric Collections
```cs
// Transform ArrayList into an IEnumerable<T>-compatible type.
var myCarsEnum = myCars.OfType<Car>();

// Create a query expression targeting the compatible type.
var fastCars = from c in myCarsEnum where c.Speed > 55 select c;
```

## Filtering Data Using OfType<T>()
```cs
// Extract the ints from the ArrayList.
ArrayList myStuff = new ArrayList();

myStuff.AddRange(new object[] { 10, 400, 8, false, new Car(), "string data" });

var myInts = myStuff.OfType<int>();

// Prints out 10, 400, and 8.
foreach (int i in myInts)
{
  Console.WriteLine("Int value: {0}", i);
}
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

## Deferred Execution
```cs

```

##Chaining Query Operators
```cs
string[] names = { "Tom", "Dick", "Harry", "Mary", "Jay" };
IEnumerable<string> query = names
.Where (n => n.Contains ("a"))
.OrderBy (n => n.Length)
.Select (n => n.ToUpper());
foreach (string name in query) Console.WriteLine (name);


//Output
JAY
MARY
HARRY
```


## Natural Ordering
```cs
int[] numbers = { 10, 9, 8, 7, 6 };
IEnumerable<int> firstThree = numbers.Take (3); // { 10, 9, 8 }


IEnumerable<int> lastTwo = numbers.Skip (3); // { 7, 6 }

IEnumerable<int> reversed = numbers.Reverse(); // { 6, 7, 8, 9, 10 }


int firstNumber = numbers.First(); // 10
int lastNumber = numbers.Last(); // 6
int secondNumber = numbers.ElementAt(1); // 9
int secondLowest = numbers.OrderBy(n=>n).Skip(1).First(); // 7

//aggregation
int count = numbers.Count(); // 5;
int min = numbers.Min(); // 6;


//quantifiers
bool hasTheNumberNine = numbers.Contains (9); // true
bool hasMoreThanZeroElements = numbers.Any(); // true
bool hasAnOddElement = numbers.Any (n => n % 2 != 0); // true

```

## Deferred Execution
The extra number that we sneaked into the list after constructing the query is
included in the result, because it’s not until the foreach statement runs that any
filtering or sorting takes place. This is called deferred or lazy execution.
```cs

var numbers = new List<int>();
numbers.Add (1);

IEnumerable<int> query = numbers.Select (n => n * 10); // Build query

numbers.Add (2); // Sneak in an extra element

foreach (int n in query)
Console.Write (n + "|"); // 10|20|









int[] numbers = { 10, 20, 30, 40, 1, 2, 3, 8 };

// Get numbers less than ten.
var subset = from i in numbers where i < 10 select i;

**// LINQ statement evaluated here!**
foreach (var i in subset)
    Console.WriteLine("{0} < 10", i);


// Change some data in the array.
numbers[0] = 4;

**// Evaluated again!**
foreach (var j in subset)
    Console.WriteLine("{0} < 10", j);

```

## Reevaluation
Deferred execution has another consequence: a deferred execution query is reevaluated when you re-enumerate.

```cs
var numbers = new List<int>() { 1, 2 };
IEnumerable<int> query = numbers.Select (n => n * 10);

foreach (int n in query) Console.Write (n + "|"); // 10|20|
numbers.Clear();

foreach (int n in query) Console.Write (n + "|"); // <nothing>

```


## Captured Variables
lambda expressions capture outer variables, the query will honor the value of the those variables at the time the query runs
```cs
int[] numbers = { 1, 2 };
int factor = 10;
IEnumerable<int> query = numbers.Select (n => n * factor);
factor = 20;
foreach (int n in query) Console.Write (n + "|"); // 20|40|

```



## Captured Variables
```cs


```



## Immediate Execution
```cs
int[] numbers = { 10, 20, 30, 40, 1, 2, 3, 8 };

// Get data RIGHT NOW as int[].
int[] subsetAsIntArray = (from i in numbers where i < 10 select i).ToArray<int>();

// Get data RIGHT NOW as List<int>.
List<int> subsetAsListOfInts = (from i in numbers where i < 10 select i).ToList<int>();

```


## Using Enumerable / Lambda Expressions
```cs

string[] currentVideoGames = {"Morrowind", "Uncharted 2",
"Fallout 3", "Daxter", "System Shock 2"};
// Build a query expression using extension methods
// granted to the Array via the Enumerable type.
var subset = currentVideoGames.Where(game => game.Contains(" "))
.OrderBy(game => game).Select(game => game);
// Print out the results.
foreach (var game in subset)
Console.WriteLine("Item: {0}", game);
Console.WriteLine();


// Break it down!
var gamesWithSpaces = currentVideoGames.Where(game => game.Contains(" "));
var orderedGames = gamesWithSpaces.OrderBy(game => game);
var subset = orderedGames.Select(game => game);




//Using Anonymous Methods
// Build the necessary Func<> delegates using anonymous methods.
Func<string, bool> searchFilter =
delegate(string game) { return game.Contains(" "); };
Func<string, string> itemToProcess = delegate(string s) { return s; };
// Pass the delegates into the methods of Enumerable.
var subset = currentVideoGames.Where(searchFilter)
.OrderBy(itemToProcess).Select(itemToProcess);
// Print out the results.
foreach (var game in subset)
Console.WriteLine("Item: {0}", game);
Console.WriteLine();


//"***** Using Raw Delegates *****"
// Build the necessary Func<> delegates.
Func<string, bool> searchFilter = new Func<string, bool>(Filter);
Func<string, string> itemToProcess = new Func<string,string>(ProcessItem);
// Pass the delegates into the methods of Enumerable.
var subset = currentVideoGames
.Where(searchFilter).OrderBy(itemToProcess).Select(itemToProcess);
// Print out the results.
foreach (var game in subset)
Console.WriteLine("Item: {0}", game);
Console.WriteLine();





```



## Using Enumerable / Lambda Expressions
```cs


```


