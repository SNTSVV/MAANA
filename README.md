# MAANA: An Automated Tool for DoMAin-specific HANdling of Ambiguity

## About MAANA
*MAANA* is a tool, associated with the ICSE technical paper -- titled **"Using Domain-specific Corpora for Improved Handling of Ambiguity in Requirements"**, for performing domain-specific handling of ambiguity in requirements. The focus of MAANA is on coordination ambiguity (CA) and prepositional-phrase attachment ambiguity (PAA). 

The tool was developed at SnT / University of Luxembourg with funding from Luxembourg's National Research Fund (FNR). 

## What is released?

We release the following artifacts: 
* **./maana-source.zip**: is a compressed version of the Maven project with the source code. Instructions about reusing this compressed version are provided below.
* **./Corpora.zip**: contains the seven domain-specific corpora that we generated for the empirical evaluation of our ICSE technical paper (mentioned above).
* **./example.zip**: includes an executable jar file (*run.jar*) for running our implementation, sample requirements (*sample.txt*) that are randomly selected from the *Satellite* domain, the groundtruth file for the sample requirements (*sample-groundtruth.csv*), and a python script for evaluating the output of MAANA (*evaluate.py*). 
* **./Data.zip**: includes the non-proprietary requirements documents which we use in our evaluation in subfolder "Documents/", as well as the corresponding ground truth in subfolder "Ground-Truth". 
In "Ground-Truth", we provide both (a) the reconciled decisions (our gold standard) derived from the the annotators' input and (b) the raw input from the individual annotators.

## How to use MAANA?  
MAANA is implemented as an [Apache Maven project](https://maven.apache.org/) and builds on the [Apache UIMA framework](http://uima.apache.org). For a comeplete list of the system requirements, see the *Requirements File*. 

### Import in Eclipse 
1. download the compressed project *maana-source.zip* to your local machine
2. Uncompress the folder to *./maana-source/*
3. In Eclipse, select *File>import ...*
4. Select *Maven>Existing Maven Projects* and click *Next>*
5. Click *Browse...* next to the *Root Directory* field 
6. Select the uncompressed folder *./maana-source/*
7. Under *Projects*, you will then see the */pom.xml* file (if not, press on *Refresh* button)
8. Select the pom file and click *Finish*
9. You will the maven project *lu.svv.saa.maana* imported to your workspace in Eclipse 

To avoid dependenncy problems, we included in *maana-source* the library jar file *ws4j-1.0.0.jar*. For importing this into your project, follow these steps:
1. right click on the project, then select *Build Path>Configure Build Path...*
2. select *Add External JARs...*
3. browse to where the library is located and click open
4. click on *Apply and Close*

### Run the jar file 

The required files for running the instructions described below are located in *example.zip*, specifically: 
* *sample.txt* is a plain text file that contains **19** requirements selected randomly from the **Satellite** domain. 
Each requirement is a single sentence on a separate line. 
* *sample-groundtruth.txt* is the corresponding ground-truth file to the sample requirements.
* *run.jar* the executable jar file that deploys a minimal version of *MAANA* tool. 
* *corpus/mini-corpus/* folder contains a minimal domain-specific corpus generated from the Satellite domain with a reduced size.  
* *evaluate.py* is a python script for evaluating the output (as explained below).

## Getting the Project

- Clone the [GitHub repository *MAANA*](https://github.com/SNTSVV/MAANA) to your local machine
- Extract the *example.zip* file
- Navigate to the *./MAANA/example* folder on your local machine
```
cd path/to/MAANA/example/
```

## Usage Example

The required files for running the instructions described below are located in example.zip.

### Running the jar file 

- For a **simple run** (running MAANA on *sample.txt*), use the following command: 
```
java -Xmx4096m -jar run.jar -doc sample.txt -corpus corpus/mini-corpus
```

- For a **customized run**, use the following command (*Note*:This command might require few minutes.)
```
java -Xmx4096m -jar run.jar -doc [PATH_TO_DOC] -corpus [CORPUS_GENERATION_OPTION]
```
Where the arguments: 
* -Xmx4096m to increase the amount of memory assigned to the JVM to 4GB.
* [PATH_TO_DOC] is the path to the document to be analyzed. In our example, the document *sample.txt* is in the same directory where the jar file is, but this can be any absolute path to a text file that contains requirements. 
* [CORPUS_GENERATION_OPTION] refers to one of the following options: 
    - *corpus/*mini-corpus**: for using a reduced version of the corpus from the Stellite domain which is necessary for running our usage example. This option works only for ``sample.txt`` document. If you want to use the entire corpus for the Satellite domain, you need to choose number 6 as elaborated next. 
    - *corpus/X*, where **X** is a number between **1 and 7**: for using our pre-generated corpora, given the domain IDs 1-7, defined in the table below. To use any of the pre-generated corpora, you need first to move or copy/paste the folder from ``./Corpora`` to the ``./example/corpus/`` directory where the jar file is located (e.g., move ./Corpora/1/ to the ./example/corpus/1/ folder to use the corpus for *Aerospace* domain on a different input requirements document (e.g., ``reqs-doc.txt``). Then, the jar file can be executed using the following command: ``java -Xmx4096m -jar run.jar -doc reqs-doc.txt -corpus corpus/1/``, assuming that the requirements document ``reqs-doc.txt`` is also located in the directory ``.\example`. 
    - The option **new-corpus**: for generating a new domain-specific corpus for the input requirements document by crawling Wikipedia. A example command for running this option looks like: ``java -Xmx4096m -jar run.jar -doc reqs-doc.txt -corpus new-corpus``. As a prerequisite for using this option, you need to set up Wikipedia (for simplicity, we moved the instructions related to setting up Wikipedia into a separate file, that is the *Additional Instructions File*). 

| ID            | Domain         |
| :-----------: | :------------: |
| 1             | Aerospace      |
| 2             | Automative     |
| 3             | Defense        |
| 4             | Digitalization |
| 5             | Medicine       |
| 6             | Satellite      |
| 7             | Security       |
    

### Output of MAANA

Once the execution is completed, one CSV file (for both ambiguity types) will be created. In the case of *sample.txt*, the output file *sample-ambiguity.csv* is generated. 

The CSV file contains the following columns:

| Column        | Content                                                                                                                                    |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| Sentence      | The full sentence that contains a potentially ambiguous segment; a sentence might be repeated multiple times if it has multiple potentially ambiguous segments.  |
| Beginning offset   | The offset of the first character in the potentially ambiguous segment (CA or PAA).  |
| Ambiguity Type     | The type of ambiguity that is analyzed in the segment.  |
| Final Decision | The computed verdict for each segment: **0** indicates Ambiguous, **1** indicates First Read for CA and Verb Attachment for PAA, **2** indicates Second Read for CA and Noun Attachment for PAA. |

The first two lines in the *sample-ambiguity.csv* should look as follows:

| Sentence                                                                                                 | Beginning Offset | Ambiguity Type | Final Decision |
| -------------------------------------------------------------------------------------------------------- | :--------------: | :------------: | -------------- |
| Service availability shall measure the outage of LEO satellites and terminals.                           | 49               | CA             | 0              |

### Evaluating the Results of MAANA 

- Note that for running the python script successfully, you need to have python 3 installed. See the *Requirements File* for more details.
- Use *evaluate.py* for evaluating any output file against the corresponding ground-truth file with the following command: 
```
    python3 evaluate.py [GROUND_TRUTH_FILE] [OUTPUT_FILE]
```
Where:
        [GROUND_TRUTH_FILE] is the *ground-truth.csv* file and 
        [OUTPUT_FILE] is the output file generated by MAANA.

- For evaluating the results on *sample.txt*, use the folloing command: 

```
    python3 evaluate.py sample-groundtruth.csv sample-ambiguity.csv 
```

The expected output on the console resulted is: 

```
* Results of CA detection: 
Precision = 88.9 %
Recall = 88.9 %

* Results of PAA detection: 
Precision = 85.7 %
Recall = 85.7 %
```

## How to cite?
If you wish to use or compare with MAANA, please cite the following paper:
     Ezzini, S., Abualhaija, S., Arora, C., Sabetzadeh, M., Briand, L. C. (2021, May). Using domain-specific corpora for improved handling of ambiguity in
requirements. In 43rd International Conference on Software Engineering (ICSE).  

## License

MAANA is licensed under MIT License.

**Copyright (c) 2021 University of Luxembourg**

Permission is hereby granted, free of charge, to any person obtaining a copy of this
software and associated documentation files (the "Software"), to deal in the Software
without restriction, including without limitation the rights to use, copy, modify,
merge, publish, distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to the following
conditions:
The above copyright notice and this permission notice shall be included in all copies
or substantial portions of the Software.
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED,
INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF
CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE
OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
