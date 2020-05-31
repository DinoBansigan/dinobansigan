Title: Tips for Working with Production Databases
Published: 05/31/2020
Tags:
   - Database
   - SQL Server
---
A few weeks ago, a co-worker unintentionally ran migration scripts on a Production database. Obviously this caused some problems and our boss had to scramble to rollback the changes. In this post, I'll share some tips for working with Production databases so the same thing doesn't happen to you.

### 1. Check your Database Connections

Double-check your database connection before running any scripts. Oftentimes, this simple check is all you need to do, to avoid accidentally deleting data in a Production database.

### 2. Start new instance of SQL Server Management Studio

When connecting to a Production database, start up a new instance of SQL Server Management Studio. The idea is to keep scripts between Production and other databases separate. This helps you distinguish between database connections. And hopefully stop you from running a script that will mess up the Production database.

### 3. Work off Database Backups

Better yet, if you have the option, restore a backup copy of the Production database on a dev box and work off that. This way, you can mess with the data and not worry about affecting Production.

### 4. Use Database Transactions

If the script you are running has the potential to change a databases' data or schema - like if it has the words ALTER, CREATE, DELETE, INSERT, UPDATE, or any other statement that can modify the contents of a database - then you should be wrapping the scripts in a transaction. BEGIN TRAN is your friend. I do this even when I'm running the scripts on a local database instance. It might sound like overkill to use transactions on scripts running on non-PROD databases, and maybe it is. But what's an extra 10 seconds or so of doing that, if it prevents you from accidentally blowing up the Production database? I do it for peace of mind. It's part of routine for me now. Anytime I write a script that has the words ALTER, CREATE, DELETE, INSERT or UPDATE, I wrap them in a transaction. Doesn't even matter what database I'm connected to.

This is how I wrap scripts in a transaction:
```
BEGIN TRAN

-- ALTER, CREATE, DELETE, INSERT, UPDATE Statements go here

ROLLBACK TRAN
COMMIT TRAN
```
I use it this way: I highlight the scripts starting from BEGIN TRAN and stop just before the ROLLBACK TRAN statement. I run those and double-check the results. This gives me the opportunity to either ROLLBACK or COMMIT depending on the results.

Also, you'll notice above that I have a ROLLBACK TRAN just before the COMMIT TRAN statement. This is just an extra safety precaution that can be especially useful when you're sending out scripts to someone else. They might accidentally run the scripts in a Production database, but the ROLLBACK TRAN will make sure nothing bad happens.

### 5. Close Instances of SQL Server Management Studio

Before going home or logging off from work, close any instances of SQL Server Management Studio, especially those that you used to connect to a Production database. You'll want to do this because closing an SSMS instance while you have an open transaction, will cause it to display a warning. An open transaction has the potential to block queries and cause timeouts. This will remind you to either rollback or commit your transactions before logging off.

That's it for today. I hope those tips are helpful. If you have your own tips for working with Production databases, do share them by leaving a comment below or sending me an email.