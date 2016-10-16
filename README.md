# SQL Style Guide

Everyone has their own way of formatting SQL, this guide tries to bring some simple rules to it.

---

### Naming

* Do - Name object with their types at the end, except tables
* Do Not - Mix and match, once you have a style stick to it
* Why - It improves allows you to see what type of object is being used at a glance

```SQL
Company --< Table
CompanyView --< View
InsertCompanyProcedure --< Procedure
GetCompanyFunction --< Function
```

* Do - Use singular names for tables
* Do Not - Use plural names
* Why - Singular names tend to be shorter and a bag is called a bag no matter how many items it holds

```SQL
	Select
			*
		From
			dbo.CompanyEmployee
```

* Do - Use fullnames for objects i.e. CompanyEmployee
* Do Not - Shorten to CompanyEmp
* Why - Whist it may make sense for the original developer, it may not for subsequent developers

```SQL
	Select
			*
		From
			dbo.CompanyEmployee
```

### Tabbing

* Do - Start each statement on a new line and tab the lines underneath
* Do Not - Bunch code together on one line
* Why - It improves readability, and breaks the code into obvious sections

```SQL
Declare
	@CompanyNo Int = 1
	, @Name nVarChar(100) = 'Company 1'
	, @CountryNo Int = 1
	, @EmailAddress nVarChar(100) = 'company@company1.com'
	, @DateIncorporated Date = '2016-10-15'
	, @IsActive Bit = 1

Insert Into
	dbo.Company
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
* Why - It improves readability, and make commenting out easier

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

### Aliases

* Do - Use Alias's for objects (they should normally be the first letter of the object it Company becomes c (lowercase))
* Do Not - Use the full object name, unless very short
* Why - If everything has an alias the engine does not have to look for the objects the columns are part of

```SQL
Select
		c.CompanyNo
	From
		dbo.Company c
```

* Do - Specify the schema the object is part of
* Do Not - Leave the schema blank
* Why - If everything has a schema the engine does not have to work out what the current one is

```SQL
Select
		c.CompanyNo
	From
		dbo.Company c
```

* Do - Place column alisas next to the column definition
* Do Not - Tab them inline further across the screen
* Why - It can be hard to tell which column aliases line up with which columns definitions, and as most don't need an alias it leaves gaps

```SQL
Select
        c.CompanyNo As Id
        , c.Name
        , c.CountryNo
        , c.EmailAddress As Email
        , c.DateIncorporated
        , c.IsActive
    From
        dbo.Company c
```

### Case Statements

* Do - Split CASE statements onto new lines
* Do Not - Set the case statement inline
* Why - It improves readability, and make commenting out easier

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

### Joins

* Do - Inline JOIN conditions unless over 3 conditions
* Do Not - Split onto new lines ever ytime
* Why - Join conditions rarely change

```SQL
Select
		c.CompanyNo
	From
		dbo.Company c
		Join dbo.Country cty On c.CountryNo = cty.CountryNo And cty.IsActive = 1
```

* Do - Use JOIN and LEFT JOIN
* Do Not - Use INNER JOIN or LEFT OUTER JOIN
* Why - Shorter form of the same statement

```SQL
Select
		c.CompanyNo
	From
		dbo.Company c
		Join dbo.Country cty1 On c.CountryNo = cty1.CountryNo
		Left Join dbo.Country cty2 On c.CountryNo = cty2.CountryNo
		Inner Join dbo.Country cty3 On c.CountryNo = cty3.CountryNo --< Do NOT
		Left Outer Join dbo.Country cty4 On c.CountryNo = cty4.CountryNo --< Do NOT
```

* Do - Inline very simple 1 select/one where
* Do Not - Inline anything more complicated
* Why - Keep the line between tidy and readable

```SQL
Select
		c.CompanyNo
		, (Select Count(*) From dbo.Company Where CountryNo = c.CountryNo) As CompanyCountryCount
	From
		dbo.Company c

Select
		c.CompanyNo
		, (
			Select
					Count(*)
				From
					dbo.Company
				Where
					CountryNo = c.CountryNo
					And c.IsActive = 1
		) As CompanyCountryCount
	From
		dbo.Company c
```

### Setting Parameter Values

* Do - Use SELECT to set property values
* Do Not - Use SET
* Why - Select can be used both for getting values from a table and setting values, keep just one

```SQL
Select
		@CompanyNo = 1
		, @EmailAddress = c.EmailAddress
		, @DateIncorporated = '2015-06-01'
	From
		dbo.Company c
	Where
		c.IsActive = 1
```

### Insert/Update

* Do - Use @RowCount to combine and UPDATE / INSERT
* Why - Simplify update / insert procedures

```SQL
Begin Tran
	Update
			c
		Set
			c.EmailAddress = @EmailAddress
		From
			dbo.Company c
		Where
			c.CompanyNo = @CompanyNo

	If @@RowCount = 0
	Begin
		Insert Into
			dbo.Company
			(
				EmailAddress
			)
			Values
			(
				@EmailAddress
			)
	End
Commit Tran
```

### Commenting

* Do - Leave a comment at the bottom of the Stored Procedure with example execution values
* Why - When testing or updating an sp having example values can make it alot easier

```SQL
Select
		@CompanyNo = 1
		, @EmailAddress = c.EmailAddress
		, @DateIncorporated = '2015-06-01'
	From
		dbo.Company c
	Where
		c.IsActive = 1
```
