Title: Telerik JustDecompile
Published: 12/13/2020 23:00:00
Tags:
   - Programming
   - Tools
---

First of all, just wanted to point out that this is not a sponsored post. Nor do I use an affiliate link. I'm just sharing my experience with a tool that has helped me a number of times with my work. So, let's get to it then. What is [Telerik JustDecompile](https://www.telerik.com/products/decompiler.aspx)? 

Telerik JustDecompile is a tool that can help you decompile .NET DLLs. What that means is that it will let you see the compiled code inside a DLL file. The compiled code won't exactly match the source code, but it is close enough that you'll have no trouble understanding it. It's very easy to use too. You simply right click a DLL file and choose open with "Telerik JustDecomplie". And the best thing about it, is that it's free. 

In my experience, the Telerik JustDecompile tool is great for helping troubleshoot build issues. I was assisting someone who was running into a WCF service error. The service method reported in the error, was nowhere to be found in the codebase. And I was able to verify that, by opening up the service DLL file on the test server, using JustDecompile. This allowed me to look at the compiled code inside that DLL. And it proved that the service method mentioned in the error, is not in the DLL. It turns out that the client app he was running, was a newer version from a different branch. That explains why the service method it was trying to call, didn't exist.