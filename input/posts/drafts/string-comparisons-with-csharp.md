Title: String Comparisons with C#
Published: 
Tags:
   - C#
   - .NET
   - Programming
---

For the past month or so, I have noticed that I've been pointing out the need to do a case-insensitive string comparison in the code reviews that I've done. In light of that, I thought now would a good time to do a refresher on string comparisons with C#.

*** Using the `==` operator ***
Doing string comparisons in C# is pretty easy and the easiest way to do it is through the use of the `==` operator. 

The main drawback with this approach is that it will only do a case-sensitive comparison.

*** Using `ToUpper()` or `ToLower()` ***

For quick case-sensitive string comparisons, you can opt to call `ToUpper()` or `ToLower()` on your strings before doing the comparison using the `==` operator.

I don't see much drawbacks to using this approach, except in the event that one of your strings is null, in which case you'll then have a NullRefException on your hands. So there is an extra step of making sure the string is not null before you call one of its methods. Which leads me to another way of doing string comparisons...

*** Using `String.Compare` ***