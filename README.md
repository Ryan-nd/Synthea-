# Synthea-## Things To Download

Prerequisites- Java 8 & PgAdmin & Rstudio & Rtools & DatabaseConnector OHDSI & Java8 Driver

- Download Synthea Version 3.0.0 in Master Branch
- Run the steps in Github (Follow the Steps)

https://github.com/synthetichealth/synthea

- Important Step- Ensure Build Test is Successful- FYI: This is to make sure the data is generated  by Synthea is correct.
- Once that is done, enter this line: `run_synthea -p 1000 --exporter.csv.export=true`

---

- Download Athena Dictionary-5 GB of Data ( Auto )

[Athena](https://athena.ohdsi.org/search-terms/start)

- Create an account, and click into ‘Download’ tab, the dictionaries are required are auto-selected for you.

*FYI: Athena Dictionary is used for semantic mapping*

---

## Conversion from Synthea CSV to OMOP CDM

- Go to ETL Synthea to install ETL

https://github.com/OHDSI/ETL-Synthea

- Follow the Steps:
    - Create Database on PgAdmin
    - Create two Schemes (Synthetic & CDM)
    - Find the path to the Vocabulary & Synthetic Data as follows. (Download the Driver for corresponding Java version).
    
           (*FYI: Ensure / becomes //)*  
    
    - In the parameters, set the Synthea version as **3.0.0** & CDM_version as **5.3**.
- You can run line by line in the guide.

## Things To Download

Prerequisites- Java 8 & PgAdmin & Rstudio & Rtools & DatabaseConnector OHDSI & Java8 Driver

- Download Synthea Version 3.0.0 in Master Branch
- Run the steps in Github (Follow the Steps)

https://github.com/synthetichealth/synthea

- Important Step- Ensure Build Test is Successful- FYI: This is to make sure the data is generated  by Synthea is correct.
- Once that is done, enter this line: `run_synthea -p 1000 --exporter.csv.export=true`

---

- Download Athena Dictionary-5 GB of Data ( Auto )

[Athena](https://athena.ohdsi.org/search-terms/start)

- Create an account, and click into ‘Download’ tab, the dictionaries are required are auto-selected for you.

*FYI: Athena Dictionary is used for semantic mapping*

---

## Conversion from Synthea CSV to OMOP CDM

- Go to ETL Synthea to install ETL

https://github.com/OHDSI/ETL-Synthea

- Follow the Steps:
    - Create Database on PgAdmin
    - Create two Schemes (Synthetic & CDM)
    - Find the path to the Vocabulary & Synthetic Data as follows. (Download the Driver for corresponding Java version).
    
           (*FYI: Ensure / becomes //)*  
    
    - In the parameters, set the Synthea version as **3.0.0** & CDM_version as **5.3**.
- You can run line by line in the guide.

### Here’s an example:

```r
cd <- DatabaseConnector::createConnectionDetails(
  dbms     = "postgresql", 
  server   = "localhost/synthea_testdata", 
  user     = "postgres", 
  password = "-------", 
  port     = 5432, 
  pathToDriver = "C:\\Users\\VM\\Desktop\\WorkSpace\\JDBCDriver"  
)

cdmSchema      <- "cdm_synthea_test"
cdmVersion     <- "5.3"
syntheaVersion <- "3.0.0"
syntheaSchema  <- "synthea_data"
syntheaFileLoc <- "C:\\Users\\VM\\Desktop\\WorkSpace\\synthea-3.0.0-international\\output\\csv"
vocabFileLoc   <- "C:\\Users\\VM\\Desktop\\WorkSpace\\Athena"

ETLSyntheaBuilder::LoadEventTables(connectionDetails = cd, cdmSchema = cdmSchema, syntheaSchema = syntheaSchema, cdmVersion = cdmVersion, syntheaVersion = syntheaVersion)
```

## Bugs to Take Note:

- Fixing LoadVocabfromCSV does not take into consideration of the LOINC code ‘NA’ , it would consider the NA values in dictionary as NULL values. (Fixed)
- Bugs to be fixed in Data(Issue):
    
    *Background: These bugs come from the data generated from Synthea:*
    
    - When using Synthea 3.0.0 for ETL, **note to change** the ‘Birth_date’ and ‘Deat’ column to Y-M-D instead of d/m/y in the **excel** files (Person & Observation & condition).
    - There will be an error that shows up ‘ ID =1’ exist in ‘Person’ & ‘Observation Date’ table. **Note to truncate** these two tables in Postgres before using ‘LoadEventstable’ from GitHub.
        - E.g. Use this command I’ve created to truncate all the tables:
- Bugs to be Fixed in Postgres (Issue)
    - Device Exposure Unique Device ID & Device Source Value, **note to change** constraint Character varying to 100.
- Environment-Related Issues (Fixed)
    - Java Environment has external environment variables that would affect the parameters of building Synthea 3.0.0. (Java Major Version 61) which Synthea would then not recognise the issue.
    - When exploring the use of  Synthea 3.1.1, it has a build error of EntityManager Class, the bug was informed and further fixed from ‘String’ to ‘Path’ baseDirectory in the code base. (For more info., find it in [EntityManagerTest.java](http://EntityManagerTest.java) line 75)
    - In meeting this error, we can try to test our gradle to see if it is the issue. As after studying on the concept behind the classes, we find out that there may be some sequence that the user is hitting that creates an invalid settings causing the test to fail.

---

## Comparing Synthea Version 3.0.0 & Synthea 3.1.1

- Patient.csv has **fipscode** and **income** in Synthea 3.1.1 NOT in Synthea 3.0.0

## More About ETL Synthea R Functions

LoadVocabfromCSV: Import data into concept, concept_ancestor, concept_class,concept_relationship,concept_synonym, domain,drug_strength,relationship,vocabulary.

## Specs That I Used with Processing The Data

AMD Ryzen 5900 Series, 16 GB RAM, 144 refresh Herz

(Take Note: With the following System, it took me 2hrs 45min to complete converting the data)
