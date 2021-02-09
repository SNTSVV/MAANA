# Installation File 

This file describes the instructions to run *MAANA* tool on a sample requirements file (*sample.txt*). 

## Introduction
The required files for running the instructions described below are located in *example.zip*, specifically: 
* *sample.txt* is a plain text file that contains **19** requirements selected randomly from the **Satellite** domain. 
Each requirement is a single sentence on a separate line. 
* *sample-groundtruth.txt* is the corresponding ground-truth file to the sample requirements.
* *run.jar* the executable jar file that deploys a minimal version of *MAANA* tool. 
* *corpus/mini-corpus/* folder contains a minimal domain-specific corpus generated from the Satellite domain with a reduced size.  
* *evaluate.py* is a python script for evaluating the output (as explained below).

## Getting the Project

- Clone the [GitHub repository *MAANA*](https://github.com/SNTSVV/MAANA) (or get it from [Zenodo](https://zenodo.org/record/4457189)) to your local machine, for example using the following command:
```
git clone https://github.com/SNTSVV/MAANA.git
```
- Extract the *example.zip* file
- Navigate to the *./MAANA/example* folder on your local machine
```
cd path/to/MAANA/example/
```

## Running MAANA using run.jar 

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
    

## Output of MAANA

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

## Evaluating the Results of MAANA 

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

## Running MAANA using the Docker container

Docker containers are small and lightweight execution environments that make shared use of the operating system kernel but otherwise run in isolation from one another. 

### Install Docker
Install Docker for Mac, Windows, or Linux from the [official Docker website](https://docs.docker.com/get-docker/).
Make sure that Docker is installed by running the command ``docker -v``

### Get MAANA image

The MAANA image has all software requirements pre-installed.
Once Docker is installed, you can pull the MAANA image using the following command:

```
docker pull ezzini/maana-ss
```
Then you can access the container using this command:

```
docker exec -it $(docker container run -d -t ezzini/maana-ss bash) bash
```

### Running MAANA from the docker container 

Navigate to MAANA folder:

```
cd MAANA
```

Run on the sample file using:

```
java -Xmx4096m -jar run.jar -doc sample.txt -corpus corpus/mini-corpus
```

The output file ``sample-ambiguity.csv`` will be created in the same directory.

### Evaluating the results

The container includes the sample ground-truth file (*sample-groundtruth.csv*) in MAANA directory. Note that only Python 3 is installed in the docker container. Thus, you can evaluate the output file (*sample-ambiguity.csv*) using this command:

```
python evaluate.py sample-groundtruth.csv sample-ambiguity.csv
```
You should get the same output as above.
