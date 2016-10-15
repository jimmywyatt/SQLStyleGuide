# SQL Style Guide

* Do - Start each statement on a new line and tab the lines underneath
* Do Not - Bunch code together on one line
* Why - It improves readablity, and breaks the code into obvious sections

```SQL
Declare
	@CompanyNo Int = 1
	, @Name nVarChar(100) = 'Company 1'
	, @CountryNo Int = 1
	, @EmailAddress nVarChar(100) = 'company@company1.com'
	, @DateIncorporated Date = '2016-10-15'
	, @IsActive Bit = 1

Insert Into
	[dbo].[Company]
	(
		CompanyNo
		, Name
		, CountryNo
		, EmailAddress
		, DateIncorporated
		, IsActive
	)
	Values
	(
		@CompanyNo
		, @Name
		, @CountryNo
		, @EmailAddress
		, @DateIncorporated
		, @IsActive
	)

Select
		c.CompanyNo
		, c.Name
		, c.CountryNo
		, c.EmailAddress
		, c.DateIncorporated
		, c.IsActive
	From
		dbo.Company c
```

* Do - Indent SELECT, FROM, WHERE clauses
* Do Not - Place the where clause inline
* Why - It improves readablity, and make commenting out easier

```SQL
Select
		c.CompanyNo
		, c.Name
		, c.CountryNo
		, c.EmailAddress
		, c.DateIncorporated
		, c.IsActive
	From
		dbo.Company c
	Where
		c.IsActive = 1
```

* Do - Indent OR or AND statments
* Do Not - Place the where clause inline
* Why - It makes it easier to see each part of the clause and comment them out as needed

```SQL
	From
		dbo.Company c
	Where
		c.IsActive = 1
		And (
			@Country Is Null
			Or c.CountryNo = @Country
		)
```

* Do - Use Alias's for objects
* Do Not - Use the full object name, unless very short
* Why - If everything has an alias the engine does not have to look for the objects the colums are part of

```SQL
Select
		c.CompanyNo
	From
		dbo.Company c
```

* Do - Specify the schema the object is part of
* Do Not - Leave the schema blank
* Why - If everything has an schema the engine does not have to work it out

```SQL
Select
		c.CompanyNo
	From
		dbo.Company c
```

* Do - Split CASE statements onto new lines
* Do Not - Set the case statement inline
* Why - It improves readablity, and make commenting out easier

```SQL
Select
		c.CompanyNo
		, Case
			When c.IsActive = 1 Then 'Active'
			Else 'InActive'
		End As Active
	From
		dbo.Company c
```
