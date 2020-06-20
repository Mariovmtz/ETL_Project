# ETL REPORT PROJECT
---

#### EXTRACTION
---
Source: Information available in the international Monetary fund (IMF), from International Financial Statistics Dataset (IFS)
Method of extraction: API Extraction from IFS branch Dataset
Format: The API returned JSONs that had to be parsed.
Data: Unemployment (thous. people), Employment (thous. people), Population (thos.people)
Functions and methodology provided in JupyterNotebook may allow to extract any other Index from the IFS Dataset for all the member countries.

#### LOAD
---

PostgreSQL was choosen as our main data repository, mainly because the data that was extracted is standarized and it doesn´t show any variation in number of fields,
however data may also be loaded in Mongdb with slight changes in our current code.
Different tables are created for every index that was choosen to be extracted, mainly to keep them organized.

It is important to note that the IMF site only allows 10 requests per second, therefore we had to include in the calling function a parameter that only allows a certain number of requests per a certain time. 

#### TRANSFORMATION
---

Few transformations on data were __before__ the data load process:
* API returned JSons per country and per index, thefore a nested loop had to be made to extract the data and create a list of tuples per index,  where a tuple represents a single observation of a given index by year and country.
* Tables were created dinamically with th name of the index.
* Timestamp field was added to every bulk insert to record the approximate time of the extraction.
* Data across all the information availabe in IMF is standarized and follows the same names and datatypes, as our data was loaded using dinamically created SQL instructions, we didn´t have the necessity to apply any data conversion analysis.

Some other transformations were applied __after__ the data load process:
* Data from our repository was loaded into a pandas dataframes (unemployment, employment, population).
* During the above process column names were renamed and dataframes were joined by country code and year.
* Laboral force was calculated using formula (number of people employed + number of people unemployed)
* Percentage of unemployment was calculated using formula (unemployed / laboral force * 100)
* Rate of change of the rate of unemployment was calculated from 1990 to 2000
* Higher change was in CF (27.45%), AR (-23.19%)
* The above dataframe was plotted in geomaps.






