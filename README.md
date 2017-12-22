CERT and GuardianProject: malware detection in Android APKs through machine learning 
===
Our project heavily involves a conference paper from 2014 titled [DREBIN: Effective and Explainable Detection of Android Malware in Your Pocket](https://www.tu-braunschweig.de/Medien-DB/sec/pubs/2014-ndss.pdf). We focus on recreating a similar situation, but adjusted for todays state-of-the-art software and test 
the effectiveness of the papers program DREBIN with modern mal- and benignware.
To achieve a meaningful result, we have to manage the following objectives:

Classification
---
The objective is to optimize a binary classification of the Android app samples. We will try a range of machine learning algorithms to discriminate between malware (positive class) and benignware (negative class). The performance of the algorithms will be compared using a ROC-curve. This curve plots the true-positive rate of the test dataset against the false-positive rate. 

The techniques used in the Drebin paper achieved a true-positive rate of 94 % at a false-positive rate of 1 %. We will try to achieve these numbers with our copy using the original dataset and possibly surpass that result. How could we surpass a true-positive rate of 94 %. There are three potential improvements of current techniques:

1. Gather more features and improve existing features (as described in the feature engineering section)
2. Use deep learning. The papers approaches scarcely made use of deep learning techniques. As our features are numerical, we can use neural networks for the task.
3. More training data. The dataset used for malware classification in the 2014 paper contained approximately 130000 labelled applications. This was considered the biggest dataset for this use at the time. Thankfully, our partners at Cert.at and the GuardianProject could provide us with considerably larger quantity of additional new applications to recognise the evolution of malware.

The new training data especially will be important to test the effectiveness of DREBIN with modern software and document the changes in the detection rate.

Feature Engineering
---
In order to supply the learning algorithms with data, we have to extract the specific properties and features of the APKs. The main obstacles in this part of the project are:

1. Finding out what we are looking for (What makes an APK malware?)
2. Knowing where to look for 

As already mentioned, the first step in this section is the reproduction of the program that extracts the features from an APK used in the DREBIN paper. Afterwards, the goal is to improve the existing feature extraction and look for new features. Interesting properties of an apk may be:

• Requested permissions
• URLs used for communication
• Method calls
• Activity names
etc...

An APK is basically a .zip file, which contains many other files, some are mandatory and some are optional. Luckily, we already have a list of APK features, which are known from the DREBIN paper to give sufficient information for the learning algorithms. The main sources of information are the Mainfest.xml (containing metadata about the app) and the classes.dex files. The .dex file contains compiled code, which must be decompiled in order to analyze it.
