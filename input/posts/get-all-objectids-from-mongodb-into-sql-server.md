Title: Get All ObjectIds From MongoDb Into SQL Server
Lead: How to quickly get all ObjectIds out of MongoDb and into SQL Server without having to write new code.
Published: 04/19/2019
Tags:
   - MongoDB
   - SQL Server
   - Scripts
   - Database
---
As software developers, one of the problems we have to be mindful of, is our tendency to "reinvent the wheel". By this I mean the tendency for developers to write a new application to solve a problem, that could have been solved by simply using existing applications/tools. When faced with the problem of importing all ObjectIds from a MongoDB collection into a SQL Server table, I thought I needed to write a new migration/utility app. Turns out I don't need to write any code at all, well except for the MongoDB query of course, but that's besides the point. Here is how I solved this problem using free tools available on the Internet.

## Solution

First up is the MongoDB query that will get you all ObjectIds from a collection. I use the free Robo 3T app to query MongoDB.
```
db.getCollection('Collection').find({},{_id:1});
```

Robo 3T has a "view results in text mode" option that will display the results in JSON format. Select that and then run the query listed above. You will then be presented with results that will look similar to what I have below. 
```
/* 1 */
{
    "_id" : ObjectId("5cba07dcca67bd08e8a6b3e2")
}

/* 2 */
{
    "_id" : ObjectId("5cba07b4ca67bc33705ded1d")
}

/* 3 */
{
    "_id" : ObjectId("5cba07baca67bc33705ded1e")
}
```

So now we have a list of ObjectIds from MongoDB. Imagine if you got back thousands of ObjectIds. It would be tempting at this point to say something like, hey I need to write a console app that can parse these results. I know I did. And I did write such an app, but eventually I realized I didn't need to. All I needed was Notepad++. *Actually any text editor with a good enough "search and replace" feature will work.*

After copying those results from Robot 3T into Notepad++, you can then use the "search and replace" functionality to transform the results into INSERT scripts that you can run in SQL Server.

So first thing you need to do is comment out all those brackets `{` and `}`. To do this, you simply search for `{` or `}` and replace the values with `--{` or `--}`. Adding `--` to the start of a line will comment it out in SQL Server. You can also opt to simply just delete all those brackets. SQL Server won't care if there are spaces between INSERT scripts. At this point your text file in Notepad++ will look similar to what I have below.
```
/* 1 */
--{
   "_id" : ObjectId("5cba07dcca67bd08e8a6b3e2")
--}

/* 2 */
--{
   "_id" : ObjectId("5cba07b4ca67bc33705ded1d")
--}

/* 3 */
--{
   "_id" : ObjectId("5cba07baca67bc33705ded1e")
--}
```

The second thing you need to do is to transform the lines with the ObjectId values in it, into SQL Server INSERT statements. To do this you go through two steps:
   1. Search for `"_id" : ObjectId("` and replace the values with `INSERT INTO [dbo].[MongoDbObjectIds] ([ObjectId]) VALUES ('`. *(This is assuming you have a MongoDbObjectIds table in SQL Server with an ObjectId column.)*
   2. Search for `")` and replace the values with `');`. This will round out the INSERT statements and at this point, you should have valid INSERT statements that can be used in SQL Server. They should look similar to the ones I have below.
   ```
   /* 1 */
   --{
      INSERT INTO [dbo].[MongoDbObjectIds] ([ObjectId]) VALUES ('5cba07dcca67bd08e8a6b3e2');
   --}

   /* 2 */
   --{
      INSERT INTO [dbo].[MongoDbObjectIds] ([ObjectId]) VALUES ('5cba07b4ca67bc33705ded1d');
   --}

   /* 3 */
   --{
      INSERT INTO [dbo].[MongoDbObjectIds] ([ObjectId]) VALUES ('5cba07baca67bc33705ded1e');
   --}
   ```

All that needs to be done now is to copy the INSERT statements, run them in SQL Server Management Studio and you are done. 

*Now it must be noted that this approach to getting ObjectIds out of MongoDB and into SQL Server is useful if you are only doing it once or twice. As soon as you have to repeat this process multiple times or often enough, then by all means write a migration/utility app to automate the process. It might take you longer to finish it, but the reusability of said migration application will pay for itself in time saved in the future.*

Hope you found this post helpful. Happy Friday to everyone!