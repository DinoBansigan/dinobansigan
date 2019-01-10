Title: SQL Server Try-Catch With Exception Handling Template
Published: 01/10/2019
Tags:
   - SQL Server
   - Scripts
   - Database
---
I'm putting this TRY CATCH template on here in case other people might find it helpful. This is what I usually use when wrapping SQL Scripts in a stored procedure. Normally I would have go to Microsoft Docs to get the syntax for using TRY CATCH and RAISERROR. Having a template that I know works and is ready to go is just easier.

```
BEGIN TRY
   BEGIN TRAN

	-- Add your scripts in here
	
   COMMIT TRAN
END TRY  
BEGIN CATCH  
	WHILE (@@TRANCOUNT > 0)
	BEGIN
		ROLLBACK TRAN;
	END

	DECLARE @ErrorMessage NVARCHAR(4000);  
	DECLARE @ErrorSeverity INT;  
	DECLARE @ErrorState INT;  

	SELECT   
		@ErrorMessage = ERROR_MESSAGE(),  
		@ErrorSeverity = ERROR_SEVERITY(),  
		@ErrorState = ERROR_STATE();  

	RAISERROR (@ErrorMessage, @ErrorSeverity, @ErrorState);  
END CATCH;
```