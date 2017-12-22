Malware detection in Android APKs through machine learning 
===
*In cooperation with CERT.at and the Guardian Project*

Our project heavily involves a conference paper from 2014 titled [DREBIN: Effective and Explainable Detection of Android Malware in Your Pocket](https://www.tu-braunschweig.de/Medien-DB/sec/pubs/2014-ndss.pdf). We focus on recreating a similar situation, but adjusted for todays state-of-the-art software and test 
the effectiveness of the papers program DREBIN with modern mal- and benignware.
To achieve a meaningful result, we have to manage the following objectives:

Feature Engineering
---
In order to supply the learning algorithms with data, we have to extract the specific properties and features of the APKs. The main obstacles in this part of the project are:

1. Finding out what we are looking for (What makes an APK malware?)
2. Knowing where to look for 

As already mentioned, the first step in this section is the reproduction of the program that extracts the features from an APK used in the DREBIN paper. Afterwards, the goal is to improve the existing feature extraction and look for new features. Interesting properties of an apk may be:

* Requested permissions
* URLs used for communication
* Method calls
* Activity names
etc...

An APK is basically a .zip file, which contains many other files, some are mandatory and some are optional. Luckily, we already have a list of APK features, which are known from the DREBIN paper to give sufficient information for the learning algorithms. The main sources of information are the Mainfest.xml (containing metadata about the app) and the classes.dex files. The .dex file contains compiled code, which must be decompiled in order to analyze it.

The result of this is a [Feature-Extractor](https://github.com/33onethird/feature-extraction)

Classification
---
The objective is to optimize a binary classification of the Android app samples. We will try a range of machine learning algorithms to discriminate between malware (positive class) and benignware (negative class). The performance of the algorithms will be compared using a ROC-curve. This curve plots the true-positive rate of the test dataset against the false-positive rate. 

The techniques used in the Drebin paper achieved a true-positive rate of 94 % at a false-positive rate of 1 %. We will try to achieve these numbers with our copy using the original dataset and possibly surpass that result. How could we surpass a true-positive rate of 94 %. There are three potential improvements of current techniques:

1. Gather more features and improve existing features (as described in the feature engineering section)
2. Use deep learning. The papers approaches scarcely made use of deep learning techniques. As our features are numerical, we can use neural networks for the task.
3. More training data. The dataset used for malware classification in the 2014 paper contained approximately 130000 labelled applications. This was considered the biggest dataset for this use at the time. Thankfully, our partners at Cert.at and the GuardianProject could provide us with considerably larger quantity of additional new applications to recognise the evolution of malware.

The new training data especially will be important to test the effectiveness of DREBIN with modern software and document the changes in the detection rate.

[Here is the repository about how the malware detection is installed and how to use it](https://github.com/33onethird/malware-test)

A website, where you can test an APK for malware was also planned.

Results
---
We have been able to use a university server with 136GB RAM and ran our program through with varying results:

### Original DREBIN data set

Method | tpr(%) | fpr(%)
--- | --- | ---
Random Forest | 95 | 0.6
SVM | |
Log. Regression | |
ANN | |

### New data set (IKARUS, Exodus, f-droid)

Method | tpr(%) | fpr(%)
--- | --- | ---
Random Forest |  | 
SVM | |
Log. Regression | |
ANN | |

tpr...true positive rate  
fpr...false positive rate  
SVM...Support Vector Machine  
ANN...Artificial Neural Network  

Out of Scope
---
#### Dynamic Analysis
We did not work with dynamic analysis, because the performance on mobile devices would decrease massively. We work exclusively with static analysis

#### Obfuscation
If the malware is being obfuscated then the decompilation would face problems, because countering obfuscation requires a whole new project and was not further included in these methods

Namesake
---
Our organization heavily revolves around the infamous [DREBIN](https://www.tu-braunschweig.de/Medien-DB/sec/pubs/2014-ndss.pdf) paper, which shares the name with a beloved Leslie Nielsen character from the filmseries "The Naked Gun". The movie lives off of slapstick and puns, much like we do.


References
---
[Arp, Daniel & Spreitzenbarth, Michael & Hübner, Malte & Gascon, Hugo & Rieck, Konrad. (2014). DREBIN: Effective and Explainable Detection of Android Malware in Your Pocket.](https://www.tu-braunschweig.de/Medien-DB/sec/pubs/2014-ndss.pdf)

[Martín García, Alejandro & Menéndez, Héctor & Camacho, David. (2017). String-based Malware Detection for Android Environments.](https://www.researchgate.net/publication/308941437_String-based_Malware_Detection_for_Android_Environments)

[Ambra Demontis, Marco Melis, Battista Biggio, Davide Maiorca, Daniel Arp, Konrad Rieck, Igino Corona, Giorgio Giacinto, Fabio Roli. (2017). Yes, Machine Learning Can Be More Secure! A Case Study on Android Malware Detection.](https://arxiv.org/pdf/1704.08996.pdf)

[Feng, Yu & Anand, Saswat & Aiken, Alex & Dillig, Isil, (2014), Apposcopy: Semantics-Based Detection of Android Malware through Static Analysis](https://www.cs.utexas.edu/~isil/fse14.pdf)

[Batyuk, Leonid & Herpich, Markus & Camtepe, Seyit & Raddatz, Karsten & Schmidt, Aubrey-Derrick & Albayrak, Sahin, (2011), Using Static Analysis for Automatic Assessment and Mitigation of Unwanted and Malicious Activities Within Android Applications](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.388.4511&rep=rep1&type=pdf)

