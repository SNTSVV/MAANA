# Additional Instructions File

In this file, we provide the instructions about setting up Wikipedia, to enable generating a new domain-specific corpus by crawling Wikipedia (see the *Installation File*). These instructions are based on the [DKPro JWPL library](https://dkpro.github.io/dkpro-jwpl/). 


## Setting up Wikipedia

1. Get an up-to-date [Wikipedia dump](https://dumps.wikimedia.org/backup-index.html). You can find a download tool for Wikipedia dumps [here](https://github.com/pacurromon/wp-download).


2. For using Wikipedia in our implementation, you need the following three files:
```
    [LANGCODE]wiki-[DATE]-pages-articles.xml.bz2 or [LANGCODE]wiki-[DATE]-pages-meta-current.xml.bz2
    [LANGCODE]wiki-[DATE]-pagelinks.sql.gz
    [LANGCODE]wiki-[DATE]-categorylinks.sql.gz
```
where [LANGCODE] is the language code, e.g. for English it is *en*, and [DATE] is the date of the dump, e.g., *20210101*.  

3. Run the transformation using *JWPLDataMachine.jar* which you can download [here](https://github.com/dkpro/dkpro-jwpl/releases/tag/de.tudarmstadt.ukp.wikipedia-1.1.0)
```
    java -jar JWPLDataMachine.jar [LANGUAGE] [MAIN_CATEGORY_NAME] [DISAMBIGUATION_CATEGORY_NAME] [SOURCE_DIRECTORY] or
    de.tudarmstadt.ukp.wikipedia.datamachine.domain.JWPLDataMachine [LANGUAGE] [MAIN_CATEGORY_NAME] [DISAMBIGUATION_CATEGORY_NAME] [SOURCE_DIRECTORY]
```    
where the parameters: 
    * LANGUAGE - a language string matching one of the [JWPL_Languages](https://dkpro.github.io/dkpro-jwpl/JWPL_Languages/).
    * MAIN_CATEGORY_NAME - the name of the main (top) category of the Wikipedia category hierarchy
    * DISAMBIGUATION_CATEGORY_NAME - the name of the category that contains the disambiguation categories
    * SOURCE_DIRECTORY - the path to the directory containing the source files

For large dumps, you need to increase the amount of memory for the JVM (e.g., to 2GB using the '-Xmx2g' as parameter). For a recent English dump, you will need at least 4GB. Also, if your system uses a different file encoding, please add -Dfile.encoding=utf8 as parameter.
        
*Note*: This is going to take some time! While the DataMachine is running, it creates three temporary *.bin* files. These can be deleted once all files in the output directory have been produced. Eventually, you will get eleven *.txt* files in the *output* subfolder.

4. Create a database with the necessary tables:
    * Make sure the database encoding is set to UTF8.
    * Create a database using command [1], Or [2], when on the mysql shell, as below. 
    * Create all necessary tables using [jwpl_tables.sql](https://github.com/dkpro/dkpro-jwpl/blob/master/de.tudarmstadt.ukp.wikipedia.wikimachine/jwpl_tables.sql) (you can copy paste the content into mysql shell)
    
    
```
    [1] mysqladmin -u[USER] -p create [DB_NAME] DEFAULT CHARACTER SET utf8; 

    [2] CREATE DATABASE [DB_NAME] DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
```
    
5. Import the data files into the database, using the following command: 

```
mysqlimport -u USER -p --local --default-character-set=utf8 {database_name} *.txt
```

6. Now, you are ready to use the database with the JWPL Core API (also see [JWPLCore:GettingStarted](https://dkpro.github.io/dkpro-jwpl/JWPLCore_GettingStarted/)). *Note*: When first connecting to a newly imported database, indexes need to be created. This takes some time (~30 minutes), depending on the server and the size of your Wikipedia dump. Subsequent connections will not have this overhead.

### Putting this together, the commands can be:
```
    java -Djdk.xml.totalEntitySizeLimit=0 -Xmx4g -jar JWPLDataMachine.jar english Contents Disambiguation ./ 
    mysql -u root -p
    MySql> CREATE DATABASE wikipedia DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci; 
    MySql> wikipedia < jwpl_tables.sql (or copy paste the .sql content in mysql command line)
    mysqlimport -u root -p --local --default-character-set=utf8 wikipedia \*.txt 
```


## Using Wikipedia in MAANA 
To use Wikipedia for generating a new corpus for an input requirements document (using the option **-corpus new-corpus** when running MAANA), you need to update the *database information* in WikipediaCorpusCreator.java class (available in the './MAANA/src/main/java/lu/svv/re/sa/WikipediaCorpusCreator.java ' folder) as follows: 

* Line 93: mysql host with port (e.g., ``localhost:3306``)
* Line 94: database name (e.g., ``wikipedia``)
* Line 95: mysql user name
* Line 96: mysql user password
* Line 97: database language (e.g., ``Language.english``)

*Note*: Our Corpus Generation module has a keyword extractor embedded into it. As stated in our submission, for keyword extraction, we use the implementation from reference [64] (Arora et al. 2017) which is available at [https://sites.google.com/site/svvregice/](https://sites.google.com/site/svvregice/)