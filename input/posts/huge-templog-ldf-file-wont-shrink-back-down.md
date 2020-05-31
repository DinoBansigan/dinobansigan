Title: Huge Templog.ldf File Won't Shrink Back Down
Published: 05/13/2020
Tags:
   - Note To Self
   - Database
   - SQL Server
---
For the past week or so, I've noticed that my C drive was constantly running out of space. Normally, I have a little over 200 GB of free space on my C drive. I ran the [SpaceSniffer](http://www.uderzo.it/main_products/space_sniffer/) tool to figure out what was taking up all the space. It pointed me to a set of temp files used by SQL Server. One file in particular, "templog.ldf", was taking up 40 GB of space. The other tempdb files were 17 GB each but there were 8 of them. My understanding is that these files should automatically shrink back down, but why won't they? 

I know what caused those files to increase in size. I've been restoring large database backups locally. And I've done it many times throughout the week. The stored procedures in those databases do use the temp tables a lot. So, I'm not surprised the SQL Server temp files got so big. But I'm still not sure why those files were not shrinking back down to their normal sizes. They maintain their size even after republishing to an empty database. If that was always the case, then I would have noticed this issue years ago. Yet, it has only been happening recently.

While I don't exactly have an explanation for why those files remained huge, I did find a way to shrink them back down. The solution was simple: restart the SQL Server service. Once I did this, the temp files went back to their regular sizes. Restarting the PC would also work, but I haven't done it in awhile since I've started working from home. I believe that contributed to the issue of files maintaining their huge sizes. So, going forward I will restart my work PC every weekend.