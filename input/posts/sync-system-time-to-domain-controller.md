Title: How to Sync System Date/Time to Domain Controller
Published: 03/06/2019
Tags: 
   - C#
   - .NET
   - Programming
   - Testing
---

Have you ever had to do testing wherein you had to move your system date/time forward or back? If so, you will probably agree that one of the annoying things is remembering to reset the system date/time back to the current date/time. Most people will manually do this, which can be tiring when done multiple times during the day. If you are working in an office environment, then your workstation's system date/time can most likely be synced to a domain controller. Here is how you can easily do that using the command prompt in Windows.

Open up the command prompt and type in the command listed below:
```
net time /domain /set /y
```
*You might have to open the command prompt in Administrator mode to get it to work.*

Taking this a step further, what if you can automatically reset the system date/time on your workstation after a test finishes? I've had to do something similar since I've had to maintain some Coded UI tests, that can change the system date/time as part of their testing. So I wrote a utility method in C# that will reset the system date/time after a Coded UI test ends. This is called via the TestCleanup method that will run after a test ends. 

```
[TestCleanup]
public void CleanUp()
{
    ResetLocalSystemTime();
}

private void ResetLocalSystemTime()
{
    using (Process netTime = new Process())
    {
        netTime.StartInfo.FileName = "NET.exe";
        netTime.StartInfo.Arguments = @"TIME /DOMAIN /SET /Y";
        netTime.StartInfo.UseShellExecute = false;
        netTime.StartInfo.CreateNoWindow = false;
        netTime.Start();

        Task.Delay(1000);
    }
}
```

Hope this helps some of the devs out there doing some testing. If you have a different way of doing this, do share them in the comments below or send it to me in an [email](/about#ContactMe).