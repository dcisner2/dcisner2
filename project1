*********************************
*Kelley Breeze and Daniel Cisneros
*ST 513 Mini-Project 1
*9/19/2022
*********************************;

*Creating a permanent library for our project data called Project1;
LIBNAME Project1 '/home/u62097541/myLib';

FILENAME WDI '/home/u62097541/myLib/WorldDevelopmentIndicators.xlsx';

PROC IMPORT DATAFILE=WDI
	DBMS= xlsx
	OUT=Project1.worldDI;
	GETNAMES=YES;
	SHEET="Data";
RUN;

*Rename the Country Name to Region, Country Code to Region_Code, Indicator Name to Indicator_Name and 
* Indicator Code to Indicator_Code;
DATA Project1.worldDI;
	SET Project1.worldDI;
	RENAME 	'Country Name'n = Region
			'Country Code'n = Region_Code
			'Indicator Name'n = Indicator_Name
			'Indicator Code'n = Indicator_Code;
RUN;

***************************************************************************************************
*EURO REGION
***************************************************************************************************;

*Select only observations where the Region_Code is EUU correponding to the European Union region
* Store this as a new dataset called EUU in our Project1 Library;

DATA Project1.EUU;
	SET Project1.worldDI;
	WHERE (Region_Code = 'EUU');
RUN; 

*********************************************
* Income per capita data for EURO REGION
*********************************************;

*Select only observations in our EUU dataset where the Indicator_Code is NY.ADJ.NNTY.PC.CD (correponding to the "Adjusted net national income per capita (current US$)")
* and create a descriptive label for our "Value" variable. Store this as a new dataset called EUUinc in our Project1 Library;

DATA Project1.EUUinc;
	SET Project1.EUU;
	LABEL Value = Adjusted net national income per capita (current US$);
	WHERE Indicator_Code = 'NY.ADJ.NNTY.PC.CD';
RUN; 


**************************************
* CO2 emissions data for EURO REGION
**************************************;

*Select only observations in our EUU dataset where the Indicator_Code is EN.ATM.CO2E.PC (correponding to the "CO2 emissions (metric tons per capita)")
* and create a descriptive label for our "Value" variable. Store this as a new dataset called EUUCO2 in our Project1 Library;

DATA Project1.EUUCO2;
	SET Project1.EUU;
	LABEL Value = CO2 emissions (metric tons per capita);
	WHERE Indicator_Code = 'EN.ATM.CO2E.PC';
RUN; 
 

**********************************************
* Urban Population data for EURO REGION
**********************************************;

*Select only observations in our EUU dataset where the Indicator_Code is SP.URB.TOTL.IN.ZS (correponding to the "Urban population (% of total population)")
* and create a descriptive label for our "Value" variable. Store this as a new dataset called EASurban in our Project1 Library;

DATA Project1.EUUurban;
	SET Project1.EUU;
	LABEL Value = Urban population (% of total population);
	WHERE Indicator_Code = 'SP.URB.TOTL.IN.ZS';
RUN; 

****************************************************************************************************************
*ASIA REGION
****************************************************************************************************************;

*Select only observations where the Region_Code is EAS correponding to the East Asia & Pacific region
* Store this as a new dataset called EAS in our Project1 Library;

DATA Project1.EAS;
	SET Project1.worldDI;
	WHERE (Region_Code = 'EAS');
RUN; 

*********************************************
* Income per capita data for ASIA REGION
*********************************************;

*Select only observations in our EAS dataset where the Indicator_Code is NY.ADJ.NNTY.PC.CD (correponding to the "Adjusted net national income per capita (current US$)")
* and create a descriptive label for our "Value" variable. Store this as a new dataset called EASinc in our Project1 Library;

DATA Project1.EASinc;
	SET Project1.EAS;
	LABEL Value = Adjusted net national income per capita (current US$);
	WHERE Indicator_Code = 'NY.ADJ.NNTY.PC.CD';
RUN; 


**************************************
* CO2 emissions data for ASIA REGION
**************************************;

*Select only observations in our EAS dataset where the Indicator_Code is EN.ATM.CO2E.PC (correponding to the "CO2 emissions (metric tons per capita)")
* and create a descriptive label for our "Value" variable. Store this as a new dataset called EASCO2 in our Project1 Library;

DATA Project1.EASCO2;
	SET Project1.EAS;
	LABEL Value = CO2 emissions (metric tons per capita);
	WHERE Indicator_Code = 'EN.ATM.CO2E.PC';
RUN; 


**********************************************
* Urban Population data for ASIA REGION
**********************************************;

*Select only observations in our EAS dataset where the Indicator_Code is SP.URB.TOTL.IN.ZS (correponding to the "Urban population (% of total population)")
* and create a descriptive label for our "Value" variable. Store this as a new dataset called EASurban in our Project1 Library;

DATA Project1.EASurban;
	SET Project1.EAS;
	LABEL Value = Urban population (% of total population);
	WHERE Indicator_Code = 'SP.URB.TOTL.IN.ZS';
RUN; 


************************************************************************************************************************************
*MERGING DATASETS OF OF THE SAME VARIBALES FOR COMPARISON
************************************************************************************************************************************;
DATA Project1.MergedINC;
	SET Project1.EUUinc Project1.EASinc;
RUN;

DATA Project1.MergedCO2;
	SET Project1.EUUCO2 Project1.EASCO2;
RUN;

DATA Project1.MergedURBAN;
	SET Project1.EUUurban Project1.EASurban;
RUN;

************************************************************************************************************************************
*USING MERGED DATASETS TO PLOT RELEVANT GRAPHS TO COMPARE
*INCOME PER CAPITA FOR EURO REGION & ASIA REGION 
************************************************************************************************************************************;

PROC SGPLOT DATA = PROJECT1.MERGEDINC;
	TITLE "Net Income Per Capita 1980-2019";
	LOESS X = YEAR Y = VALUE/
						GROUP = REGION_CODE;
RUN;

PROC MEANS DATA = PROJECT1.MERGEDINC MEAN VAR STDDEV MIN Q1 MEDIAN Q3 MAX MAXDEC = 2;
	TITLE "Net Income Per Capita 1980-1999";
	CLASS REGION_CODE;
	VAR Value;
	WHERE (YEAR BETWEEN 1980 AND 1999);
RUN;

PROC MEANS DATA = PROJECT1.MERGEDINC MEAN VAR STDDEV MIN Q1 MEDIAN Q3 MAX MAXDEC = 2;
	TITLE "Net Income Per Capita 2000-2019";
	CLASS REGION_CODE;
	VAR Value;
	WHERE (YEAR BETWEEN 2000 AND 2019);
RUN;

************************************************************************************************************************************
*USING MERGED DATASETS TO PLOT RELEVANT GRAPHS TO COMPARE AND CONTRAST
* CO2 emissions data FOR EURO REGION & ASIA REGION 
************************************************************************************************************************************;

PROC SGPLOT DATA = PROJECT1.MergedCO2;
	TITLE "CO2 Emissions Per Capita 1980-2019";
	LOESS X = YEAR Y = VALUE/
						GROUP = REGION_CODE;
RUN;

PROC MEANS DATA = PROJECT1.MergedCO2 MEAN VAR STDDEV MIN Q1 MEDIAN Q3 MAX MAXDEC = 2;
	TITLE "CO2 Emmisions per Capita 1980-1999";
	CLASS REGION_CODE;
	VAR Value;
	WHERE (YEAR BETWEEN 1980 AND 1999);
RUN;

PROC MEANS DATA = PROJECT1.MergedCO2 MEAN VAR STDDEV MIN Q1 MEDIAN Q3 MAX MAXDEC = 2;
	TITLE "CO2 Emmisions per Capita 2000-2019";
	CLASS REGION_CODE;
	VAR Value;
	WHERE (YEAR BETWEEN 2000 AND 2019);
RUN;


************************************************************************************************************************************
*USING MERGED DATASETS TO PLOT RELEVANT GRAPHS TO COMPARE AND CONTRAST
* CO2 emissions data FOR EURO REGION & ASIA REGION 
************************************************************************************************************************************;

PROC SGPLOT DATA = PROJECT1.MergedURBAN;
	TITLE "Urban Population 1980-2019";
	LOESS X = YEAR Y = VALUE/
						GROUP = REGION_CODE;
RUN;

PROC MEANS DATA = PROJECT1.MergedURBAN MEAN VAR STDDEV MIN Q1 MEDIAN Q3 MAX MAXDEC = 2;
	TITLE "Urban population (% of total population) 1980-1999";
	CLASS REGION_CODE;
	VAR Value;
	WHERE (YEAR BETWEEN 1980 AND 1999);
RUN;

PROC MEANS DATA = PROJECT1.MergedURBAN MEAN VAR STDDEV MIN Q1 MEDIAN Q3 MAX MAXDEC = 2;
	TITLE "Urban population 2000-2019";
	CLASS REGION_CODE;
	VAR Value;
	WHERE (YEAR BETWEEN 2000 AND 2019);
RUN;



************************************************************************************************************************************
*DATA SET CONTAINING ALL VARIABKES FOR EURO REGION TO DETERMINE IF THERE IS CORRELATIONS
************************************************************************************************************************************;
DATA Project1.EUUALL;
	SET Project1.EUU;
	WHERE Indicator_Code IN  ("NY.ADJ.NNTY.PC.CD" , "EN.ATM.CO2E.PC" , "SP.URB.TOTL.IN.ZS");
RUN; 

**
*TEST CODE TRYING TO FIND HOW TO SHOW CORRELATION B/W EMISSIONS/ INCOME
************;
PROC SGPLOT DATA = PROJECT1.EUUALL;
	Scatter X = YEAR Y = VALUE/ group=Indicator_code;
RUN;

PROC CORR DATA = PROJECT1.EUUALL COV;
	VAR Indicator_code YEAR ;
RUN;
