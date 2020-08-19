Title: WCF BindingConfiguration Error
Published: 08/18/2020
Tags:
   - WCF
   - C#
   - .NET
   - Programming
---
A week ago I was running a test and kept running into a WCF BindingConfiguration Error. I would not allow myself to check-in my code unless my tests passed. So, I battled with this error for over an hour.

> The binding at system.serviceModel/bindings/basicHttpBinding does not have a configured binding named ‘BasicHttpBinding_IXXXXXX’. This is an invalid value for bindingConfiguration.

*It's your basic, run of the mill, WCF BasicHttpBinding configuration error.*

I double-checked my test project's app.config file and the relevant client.config files - everything looked right. I know it was a configuration issue. The error message itself made it obvious that it was a configuration issue. But everything looked right. I didn't see any issues with the config files I was looking at.

*It must be noted that it was already close to midnight at this point. I was working late that day and decided to push myself by resolving to check-in my changes before the night ends. I have no doubt that being tired and sleepy didn't help.*

Anyway, after awhile I realized that the service method that I was trying to test, was running inside another service. So, in addition to the test project’s app.config file, I also needed to double-check that other service’s Web.config file. When I ran into this error earlier that night, I actually updated that other Web.config file. I added the entry it needed just to cover my bases. I didn't know then that it was the Web.config file that was causing the error. 

For some reason, possibly due to fatigue and needing sleep, I didn't think about checking that Web.config file again. Out of frustration, I decided to take a break and headed to the kitchen to drink a glass of water. After hanging out in the kitchen for a bit, then listening to some good music, I finally had the bright idea to check this other Web.config file again. And there it was, a typo on the BasicHttpBinding entry I added. Can't believe I didn't check on it sooner.

Lesson learned here is to take a break whenever you're stuck with a problem. Give your mind time to rest. Chances are, your subconscious will kick in and tell you what to try next. And if the problem points to a configuration issue, with WCF, it most likely is. So, check all the config files, again.