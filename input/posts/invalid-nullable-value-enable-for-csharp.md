Title: Invalid Nullable Value - Enable for C# 7.3
Published: 04/22/2020
Tags:
   - Note To Self
   - Programming
   - Visual Studio
---
I was playing around with Visual Studio 2019 and the open source ASP.NET Core blog engine [Miniblog.Core](https://github.com/madskristensen/Miniblog.Core). I cloned it to my local, opened it in Visual Studio 2019 and tried to build the solution. I immediately ran into the error below:

`Error	CS8630	Invalid 'nullable' value: 'Enable' for C# 7.3. Please use language version '8.0' or greater.`

So, I looked up the error message and every Stack Overflow page I ended up on, says that I have to set the Language Version in Visual Studio. Okay, so how do I do that? I eventually found [this answer](https://stackoverflow.com/a/52527257/5041911), which gives you the steps to get to the Advanced Build Settings dialog box for the project.

> - Right-click YourProject, click Properties
> - Click Build if it's not already selected
> - Change Configuration to All Configurations
> - Click Advanced...
> - Change the language version

When the dialog box showed up however, it didn't give me an option to set the Language Version.

![Advanced Build Settings](/assets/images/VS2019-AdvancedBuildSettings.jpg "Advanced Build Settings")

## Solution

The solution I ended up with involved manually editing the project file and adding an entry for the Language Version. So, I opened up the Miniblog.Core.csproj file and added `<LangVersion>preview</LangVersion>` under the `<PropertyGroup>` settings. It looks like this:

![Setting the Language Version manually](/assets/images/LangVersion.jpg "Setting the Language Version manually")

