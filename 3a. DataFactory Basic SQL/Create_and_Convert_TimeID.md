
TimeID from DateTime (replace GetDate with your Datetime Column)
````SQL
SELECT CONVERT(nvarchar(10), GetDate(),112)
````
Datetime from TimeID (you must cast TimeID as NVARCHAR)
````SQL
SELECT CONVERT(Datetime,CAST(TimeID AS NVARCHAR(10)))
````

Increment TimeID by 7 Days
````SQL
SELECT CONVERT(nvarchar(10),DATEADD(d,7,CONVERT(Datetime,CAST(TimeID AS NVARCHAR(10)))),112)
````

Check if TimeID is weekday
````SQL
IIF(Datepart(weekday,CONVERT(Datetime,CAST(TimeID AS NVARCHAR(10))))<=5,1,0)  AS Day_is_Weekday
````
Liste of TimeIDs per Month for given Years 2017-2018
````SQL
IF OBJECT_ID('tempdb..#Jahre') IS NOT NULL DROP TABLE #Jahre
CREATE TABLE #Jahre (Jahr INT) 
	INSERT INTO #Jahre VALUES (2017),(2018)

IF OBJECT_ID('tempdb..#Monate') IS NOT NULL DROP TABLE #Monate
CREATE TABLE #Monate (Monat INT) 
	INSERT INTO #Monate VALUES (1),(2),(3),(4),(5),(6),(7),(8),(9),(10),(11),(12)

IF OBJECT_ID('tempdb..#Monatsliste') IS NOT NULL DROP TABLE #Monatsliste;

CREATE TABLE #Monatsliste (
	TimeID BIGINT NOT NULL
	)

INSERT INTO #Monatsliste
	SELECT Jahr * 10000 + Monat*100 + 15 AS TimeID FROM #Jahre LEFT JOIN #Monate ON 1=1
````