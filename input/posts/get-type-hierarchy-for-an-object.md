Title: Get Type Hierarcy for an Object
Lead: Getting a list of Base Classes/Types for an Object in C#
Published: 02/06/2019
Tags:
   - C#
   - .NET
   - Programming
---

If you've ever needed to get a list of the base classes/types for an object in C#, this is one way of doing it. In my case, I had an object which was of the base class/type, but it was really a derived type. 

*Example:   
I had an object of type Animal, but it was really an instance of the Dog class, which is derived from the Animal base class.*

The beauty of inheritance in object oriented programming is that the current instance of the object, can be an instance of the derived type, or the base type; it doesn't matter. As long as the code expects you to provide it an object of the base type, you can provide an instance of either one. 

I needed to record the type hierarchy for that object when saving it to the database. So this utility method is what I came up with in short order. I am returing a list of strings in my example below, but there is nothing stopping you from returning an array of Types or whatever else you may need based on your scenario.

```
private List<string> GetTypeHierarchy()
{
    List<string> typeHierarchy = new List<string>();

    Type currentType = this.GetType();
    typeHierarchy.Add(currentType.Name);

    Type baseType = currentType.BaseType;
    while (baseType != null)
    {
        typeHierarchy.Add(baseType.Name);

        baseType = baseType.BaseType;
    }

    typeHierarchy.Reverse();

    return typeHierarchy;
}
```

I intentionally wrote this without recursion, because recursion hurts my head haha. There might be a better/cleaner way of doing this, if so, do share your solution with me. Share it in the comments below or send it to me in an [email](/about#ContactMe).