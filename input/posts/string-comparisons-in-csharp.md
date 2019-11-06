Title: String Comparisons in C#
Published: 11/06/2019
Tags:
   - C#
   - .NET
   - Programming
---

For the past month or so, I have noticed that I've been pointing out the need to do case-insensitive string comparisons in the code reviews that I've done. In light of that, I thought now would be a good time to do a refresher on string comparisons in C#.

### Using the `==` operator
Doing string comparisons in C# is easy, and the easiest way to do it, is with the `==` operator.
```
public static void Main()
{
   string userName1 = "dino";
   string userName2 = "dino";
   bool areStringsEqual = userName1 == userName2;
   Console.WriteLine(areStringsEqual.ToString());
}
```
Result:
`True`

The main drawback to this approach is that it will only do a case-sensitive comparison. So, any typo like in the sample code below, will not work.
```
public static void Main()
{
   string userName1 = "dino";
   string userName2 = "dinO";
   bool areStringsEqual = userName1 == userName2;
   Console.WriteLine(areStringsEqual.ToString());
}
```
Result:
`False`

So, let's look at another approach to comparing strings...

### Using `ToUpper()` or `ToLower()`

For quick case-insensitive string comparisons, you can call `ToUpper()` or `ToLower()` on your strings before doing the comparison using the `==` operator.
```
public static void Main()
{
   string userName1 = "dino";
   string userName2 = "DiNo";
   bool areStringsEqual = userName1.ToUpper() == userName2.ToUpper();
   Console.WriteLine(areStringsEqual.ToString());
}
```
Result:
`True`

I don't see much drawbacks to using this approach, except if one of your strings is null, in which case you'll then have a NullReferenceException on your hands, like in the sample code below.
```
public static void Main()
{
   string userName1 = "dino";
   string userName2 = null;
   bool areStringsEqual = userName1.ToUpper() == userName2.ToUpper();
   Console.WriteLine(areStringsEqual.ToString());
}
```
Result:
`System.Reflection.TargetInvocationException: Exception has been thrown by the target of an invocation. ---> System.NullReferenceException: Object reference not set to an instance of an object.`

*Note: The exception message above might look weird, but that is because I am running the sample code on [TryDotNet](https://try.dot.net/).*

So, there is an extra step of making sure the strings are not null before you call one of its methods, like in the sample code below.
```
public static void Main()
{
   string userName1 = "dino";
   string userName2 = null;
   bool areStringsEqual = userName1 != null && userName2 != null && (userName1.ToUpper() == userName2.ToUpper());
   Console.WriteLine(areStringsEqual.ToString());
}
```
Result:
`False`

The sample code above is still very much usable code, but having to do the null checks all the time can be tiring, which leads me to another way of doing string comparisons...

### Using `String.Compare`

Using the static `Compare` method from the `String` class allows you to easily do a case-sensitive or case-insensitive string comparison using the `ignoreCase` parameter, like in the sample codes below.

Case-insensitive comparison:
```
public static void Main()
{
   string userName1 = "dino";
   string userName2 = "DiNo";
   bool areStringsEqual = String.Compare(userName1, userName2, ignoreCase: true) == 0;
   Console.WriteLine(areStringsEqual.ToString());
}
```
Result:
`True`

Case-sensitive comparison:
```
public static void Main()
{
   string userName1 = "dino";
   string userName2 = "DiNo";
   bool areStringsEqual = String.Compare(userName1, userName2, ignoreCase: false) == 0;
   Console.WriteLine(areStringsEqual.ToString());
}
```
Result:
`False`

The beauty of using the `Compare` method is that I don't even have to do null checks. So, taking the sample code above where I had to do null checks, and replacing it with a call to `String.Compare`, results in the much cleaner sample code below.
```
public static void Main()
{
   string userName1 = "dino";
   string userName2 = null;
   bool areStringsEqual = String.Compare(userName1, userName2, ignoreCase: true) == 0;
   Console.WriteLine(areStringsEqual.ToString());
}
```
Result:
`False`

The only drawback to this approach is that it might be harder to *read* the first time around, because the `String.Compare` method returns an `int`, instead of a `boolean` value. 

Normally when comparing strings, we are asking the question, are the strings equal? True or False? So, we are expecting a `boolean` value back. With the use of `String.Compare` method, you must compare the results to 0 to determine if the strings are equal. It might take a while to get used to, but the fact that you can throw any string object at this method and not worry about null checks and typos, to me that makes it the superior approach to string comparisons.

### Which One Do I Use the Most?

When developing prototype applications or generally when I'm just testing something, I am fine with using the `ToUpper()` or `ToLower()` approach. When I'm guaranteed that none of the strings will be null, I would use the `ToUpper()` or `ToLower()` approach. Any other scenario, I prefer to use the `String.Compare` method.