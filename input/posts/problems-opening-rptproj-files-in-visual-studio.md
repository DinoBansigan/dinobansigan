Title: Problems Opening .RPTPROJ Files in Visual Studio
Published: 12/06/2020
Tags:
   - Visual Studio
---

Ran into an issue where I could not open rptproj files with Visual Studio 2019. Since these were old SSRS reports, I thought I’d try opening them with Visual Studio 2013. Didn’t work either. Visual Studio 2012? Nope.

Did some more research and eventually found out that this is not a Visual Studio issue. Apparently, to open rptproj files with Visual Studio, you need to install the corresponding Microsoft SQL Server Data Tools - Business Intelligence package. Would not have figured that out had it not been for this [post](https://www.spiria.com/en/blog/web-applications/visual-studio-does-not-recognize-rptproj-files/#:~:text=%27YourProject.rptproj%27%20cannot%20be%20opened%20because%20its%20project%20type,a%20version%20that%20supports%20this%20type%20of%20project.).

So I went [here](https://www.microsoft.com/en-in/download/details.aspx?id=36843) to download the SQL Server Data Tools BI package. Installed it. And now I can open the SSRS solution/project in Visual Studio 2012.