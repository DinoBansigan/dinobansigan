Title: Problems with Simplifying Object Initializations
Lead: Simplifying Object Initializations Can Make Troubleshooting Exceptions Harder
Published: 05/15/2019
Tags:
   - C#
   - .NET
   - Programming
---

This is a problem I've seen in the past and just recently actually, where exceptions are made harder to troubleshoot, because of the way objects are instantiated. Allow me to explain. 

```
class Program
{
    static void Main(string[] args)
    {
        try
        {
            UserDto dto = new UserDto()
            {
                UserName = "Apatosaurus",
                LastLoginDate = null
            };

            User user = new User()
            {
                UserName = dto.UserName,
                LastLoginDate = dto.LastLoginDate.Value.ToString("MM-dd-yyyy")
            };

            Console.WriteLine("User Info:");
            Console.WriteLine("Username: " + user.UserName);
            Console.WriteLine("LastLoginDate: " + user.LastLoginDate);
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
            Console.WriteLine(ex.StackTrace);
        }
    }
}

class User
{
    public string UserName { get; set; }
    public string LastLoginDate { get; set; }
}

class UserDto
{
    public string UserName { get; set; }
    public DateTime? LastLoginDate { get; set; }
}
```
Take for example the code above. What happens if the `dto.LastLoginDate` property is actually null when instantiating a `User` object? 

```
at System.ThrowHelper.ThrowInvalidOperationException(ExceptionResource resource)
at System.Nullable`1.get_Value()
at NullRefErrorExample.Program.Main(String[] args) in C:\Users\dbansigan\source\repos\NullRefErrorExample\NullRefErrorExample\Program.cs:line 21
```
The error message above is what was displayed in the console app I was running. Line 21 points to `User user = new User()`, which is the line of code that was instantiating an object, but not the line of code that caused the exception. It should have pointed to Line 24 instead, `LastLoginDate = dto.LastLoginDate.Value.ToString("MM-dd-yyyy")`. However, since I followed Visual Studio's suggestion (*IDE0017 Object initialization can be simplified*), to simplify object initialization, this is what happened. So now you can see how it can make troubleshooting a more time-consuming task. 

*Note that this is a very simple example. Imagine if you had big class that had like 20 properties and multiple lines of initialization code that could cause an exception? It would be pretty hard to figure out which line of code caused the exception without having to debug the application.*

### So how do we fix it? Should we even still try to simplify object initialization? 
Yes, we can still simplify object initializations, however we just have to be wary of using code that can cause exceptions when assigning property values. So the rule that I follow is that any line of code that could throw a null reference exception, or any exception for that matter, is taken out of the code that simplifies object initialization; they are instead moved to their own line.

So, taking the example from above, this is how I would initialize the object with the rule in mind. I basically just take out the code that assigns the value to the `user.LastLoginDate` property and move it outside the brackets containing the simplified initialization code. 
```
User user = new User()
{
    UserName = dto.UserName
};
user.LastLoginDate = dto.LastLoginDate.Value.ToString("MM-dd-yyyy");
```

I run the app once again and this is what the stack trace tells me.
```
at System.ThrowHelper.ThrowInvalidOperationException(ExceptionResource resource)
at System.Nullable`1.get_Value()
at NullRefErrorExample.Program.Main(String[] args) in C:\Users\dbansigan\source\repos\NullRefErrorExample\NullRefErrorExample\Program.cs:line 25
```
Notice how the stack trace now points to Line 25 `user.LastLoginDate = dto.LastLoginDate.Value.ToString("MM-dd-yyyy");`, which is exactly the line of code that caused the exception. So now I get a head start on troubleshooting the exception. Also, any experienced developer will be able to tell almost immediately what caused the exception, if all they had to do was look at 1 line of code.

### Are there alternative solutions to this?

An alternative is to check for null before even considering instantiating an object, like in the code below. I think this is a valid solution and it allows you to throw an exception with a detailed/specific message, or throw a custom exception. That said, if you are throwing an exception only because you cannot instantiate an object, then I don't see how it is significantly better than just letting it error out on the line of code assigning the property value. Your application is not going anywhere anyway, since you still cannot instantiate the object you need for your application to move forward. 
```
if (!dto.LastLoginDate.HasValue)
{
    throw new NullReferenceException("dto.LastLoginDate cannot be null");
}
```

*Another possible alternate solution could come from the use of nullable reference types in C# 8. However, this is something I have not been able to play with yet, so I cannot comment on the use of it.*

I've seen this problem occur on other types of code as well, like for instance code where method chaining happens. If there is an exception, the end result is the same; you can hardly tell which method parameter, or which method call, or which specific part of the code caused the exception, because they are all essentially just one line of code. In situations like this, it is often advisable to break up the chain of method calls, unless you are absolutely sure that it can never cause an exception.

I'm curious to think if other developers also instantiate objects in this manner to avoid exceptions when creating objects. If you do, or you don't, I'm curious to hear your reasons for doing so. Please do share them in the comments or send me an email so we can discuss.