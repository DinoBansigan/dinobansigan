Title: Single Responsibility Principle
Published: 01/22/2020
Tags:
   - C#
   - .NET
   - Programming
---

In this post, I'll share my thoughts regarding the use of the [Single Responsiblity Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle) based on my experiences with it.

The Single Responsiblity Principle is one of the five design principles listed in [SOLID](https://en.wikipedia.org/wiki/SOLID). Gary McLean Hall in his book *"Adaptive Code via C#"* says, 

> "The single responsibility principle instructs developers to write code that has one *and only one* reason to change. If a class has more than one reason to change, it has more than one responsibility. Classes with more than a single responsibility should be broken down into smaller classes, each of which should have only one responsibility and reason to change."

It is my favorite principle out of the five because of two reasons:

### 1. Easy to Implement

First, it is very easy to implement. You don't have to be an experienced software developer to start implementing the single responsibility principle in your code. All you need to keep in mind when writing code for a new class or sometimes even a method is, *this block of code should be responsible for only one thing.*

Are you writing code for a class to parse text files? Then make sure all the code in that class is oriented towards parsing text files. Are you writing code for a class that is supposed to send email notifications? Then keep the code focused on sending email notifications. 

### 2. Better Code

Second reason is that it will always make your code better. Better in the sense that it helps decouples your classes/methods, which reduces the potential for bugs when refactoring code. This in turn also makes your code easier to test. In my experience, I have never seen code that was made worse by implementing the single responsibility principle. 

### Drawbacks?

The only argument or drawback I've heard to the use of this principle, is the possibility of increased maintenance of the code. Since each class is responsible for doing only one thing, you could end up with a lot more classes, methods, files and projects in your solution. Still, in my opinion, that is preferable if it means I have stable code that is less prone to bugs, than to have code that is low maintenance but is susceptible to bugs.

There is also the chance that a developer will try to implement this principle on a very simple app that doesn't need to change after it's been written. In this case, trying to implement the single responsibility principle will probably take more time and add complexity to the code. *(I haven't exactly ran into a situation like this with this principle, but I think that's a possibility.)* Sometimes you will have to pick and choose where and when to implement this principle, and that can only come with experience.


### Example 
Let me go through an example where I think the use of the single responsibility principle would help.

Say you have a business rule that needs to be implemented for various products. Say that Product A, B and C all use this business rule. One thing I've seen in my experience is that if the business rule logic is similar between Products A, B or C, some developers will opt to create just one class to implement the business rule for all products. Again the argument here is that since the business logic is the same, it is easier to maintain just one class for the specific business rule. Okay, fair enough. As long as the business rule doesn't have to change between any of the products, having one class/method for the business rule is acceptable. *It is in violation of the single responsibility principle, but if the business rules won't ever change, then it is an acceptable implementation.*

But what if the business rules does change for one product, say for Product C? Now you'll have to modify the one class that all products use, to accomodate changes for just one product. What this means is that while working on the new changes, you have an increased chance of introducing bugs to the rest of the products. This also means that any changes to that business rule for any one product, means you have to perform testing for all other products affected by the changes. That is extra work that could have been avoided by following the single responsibility principle.

Now what if you have to add new products that will also have the same business rule, except the new products have slightly different business rule logic in them? Will you modify the existing class to accomodate changes for the new products? If so, you'll run into the same issue above. Adding new products will almost feel like a way to introduce bugs to your code. You can avoid that by following the single responsibility principle.

With the use of the single responsibility principle, each product would have one class that implements a specific business rule. Any changes to the business rules for one product, will not affect the other products. Which means going forward, you only need to worry about testing the changes for the products that changed or the products that were added. Everything else should keep working the same way they've been working before. Stress free coding with minimal chances of introducing bugs, just the way I like it.