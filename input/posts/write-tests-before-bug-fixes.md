Title: Write Tests Before Bug Fixes
Published: 10/24/2019
Tags:
   - C#
   - .NET
   - Programming
   - Testing
---

What is the best way to prove that your bug fix works? Well, you test it. How do you make sure your bug fix keeps working years into the future? You write a test for it.

Specifically, you want to write a test that can reproduce the bug. Only once you've been able to do this, do you actually try to fix the bug. There are several of benefits for using this approach to fixing bugs:
- First, you don't have to manually go through the steps to reproduce the bug just to see if your fix works. Sure, you need to do a manual test to confirm that it works, but ideally you do this after you're done coding most of the changes. Having to do a manual test every single time you make a small code change will consume a lot of your time. Time that is better spent writing code and fixing more bugs.

- Second, when working on another bug fix in the same class or in the same part of the application as a previous bug fix, an existing test will let you know if your new fix breaks an existing bug fix. Actually, this benefit is not restricted to just *"previous bug fixes"*. If you have tests, that cover the behavior of your whole application, then it is easier to verify if your new code changes break existing functionality.

- Third, tests can be another way of documenting the behavior of an application. If you have a test that is named `CustomerManager_Cannot_Save_Customer_Without_DriversLicenseNumber`, that immediately tells me one of the requirements needed when saving Customer data. It also tells me what class or service is used to save Customer data. If I was a new developer and I was trying to learn how the system saves a Customer record, this test is a gateway to learning about that process. *Having new developers go through tests is actually a great way to have them learn a new application/system.*

Depending on the application or system you are working, writing tests might not be easy or might take too much time. If so, then that is a hint that the application or system you are working on might be so tightly coupled, that you cannot write a small *"test"* to test a specific part of the code. You should look into decoupling classes and making their methods smaller and modular, to make it easier to test various parts of the application. If you want to learn more about making your code easier to test, I suggest learning about [SOLID Principles](https://en.wikipedia.org/wiki/SOLID) and looking into [Test-Driven Development](https://en.wikipedia.org/wiki/Test-driven_development).

When I started out working professionally as a software developer, I actually didn't write any tests. I would just write code and test my changes manually. Nowadays, I feel uncomfortable about any code changes that I don't have tests for.