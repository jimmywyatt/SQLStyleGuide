# SQL Style Guide

* Do - Start each statement on a new line and tab the lines underneath
* Do Not - Bunch code together on one line
* Why - It makes it easier to see where each statements start and ends

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
