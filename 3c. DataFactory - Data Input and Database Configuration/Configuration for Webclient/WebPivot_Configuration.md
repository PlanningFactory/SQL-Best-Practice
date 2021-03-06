
### How to use the pivot view in the Webclient

1. This script creates six standard pivot datasources for the Webclient
1. you can also use any other procedure for the Webclient, this procedure must have the two parameters @Username and @FactoryID as leading parameters
1. if the used procedure has more parameters (like ProductlineID etc.) this must be declared as optional with standard parameter in the procedure (... @ProductlineID NVARCHAR(255)='')

````SQL
/* 
	This script creates five standard pivot table configurations for the webclient 
	in all factories execpt ZT. 
	02/2017 Gerd Tautenhahn for saxess-software gmbh
	DataFactory 4.0
*/
DECLARE @FactoryKey BIGINT
DECLARE @FactoryID nvarchar(255)

DELETE FROM dbo.sx_pf_gFactories WHERE PropertyID IN ('Pivot_1','Pivot_2','Pivot_3','Pivot_4','Pivot_5','Pivot_6')

DECLARE MyCursor CURSOR FOR

    -- For all Factories exept ZT
    SELECT FactoryKey,dF.FactoryID FROM dbo.sx_pf_dFactories dF WHERE dF.FactoryID <> 'ZT'

OPEN MyCursor
FETCH MyCursor INTO @FactoryKey, @FactoryID
WHILE @@FETCH_STATUS = 0
    BEGIN
		-- First Pivot Table
		INSERT INTO dbo.sx_pf_gFactories
		(FactoryKey
		,FactoryID
		,PropertyID
		,PropertyName
		,CommentUser
		,CommentDev
		,Unit
		,ValueText
		,ValueInt
		,Scale
		,IsROSystemProperty
		,FormatID
		)
		VALUES  
			-- First Pivot Table
			(@FactoryKey, @FactoryID, N'Pivot_1', N'sx_pf_DATAOUTPUT_FactorySummary', N'Summary', N'', N'', N'', 0, 0, 0, N''),

			-- Second Pivot Table
			(@FactoryKey, @FactoryID, N'Pivot_2', N'sx_pf_DATAOUTPUT_Profit', N'Profit', N'', N'', N'', 0, 0, 0, N''),

			-- Third Pivot Table
			(@FactoryKey, @FactoryID, N'Pivot_3', N'sx_pf_DATAOUTPUT_CashValues', N'Cash', N'', N'', N'', 0, 0, 0, N''),

			-- Fourth Pivot Table
			(@FactoryKey, @FactoryID, N'Pivot_4', N'sx_pf_DATAOUTPUT_Balance', N'Balance', N'', N'', N'', 0, 0, 0, N''),

			-- Fifth Pivot Table
			(@FactoryKey, @FactoryID, N'Pivot_5', N'sx_pf_DATAOUTPUT_TextValues', N'Text', N'', N'', N'', 0, 0, 0, N''),
					
			-- Sixth Pivot Table
			(@FactoryKey, @FactoryID, N'Pivot_6', N'sx_pf_DATAOUTPUT_NumericValues', N'Numeric', N'', N'', N'', 0, 0, 0, N'')

		FETCH MyCursor INTO  @FactoryKey, @FactoryID
END
CLOSE MyCursor
DEALLOCATE MyCursor

````
