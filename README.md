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

### Usage Example

For a usage example, see the *Installation File*.

### Installation Requirements

For more details, see the *Installation-Requirements File*.

## How to cite?
If you wish to use or compare with MAANA, please cite the following paper:
     Ezzini, S., Abualhaija, S., Arora, C., Sabetzadeh, M., Briand, L. C. (2021, May). Using domain-specific corpora for improved handling of ambiguity in
requirements. In 43rd International Conference on Software Engineering (ICSE).  

## License

MAANA is licensed under MIT License.

## Setting-up Wikipedia

For instructions about how to use Wikipedia in our project, see the Additional Instruction File.

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
