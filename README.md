# multilabel-class-modeling

If unable to access view .ipynb file, use the following link to view script: https://nbviewer.jupyter.org/github/aylindincer/multilabel-class-modeling/blob/main/Multilabel_Classification_Predicting_Keywords.ipynb

General Description:

The purpose of this project is to see if it's possible to predict keywords of a scientific paper based on the abstract text.

Publications related to alzheimer's disease was pulled from https://www.ncbi.nlm.nih.gov/pubmed/ and the search criteria was set so all items needed to have an abstract and the associated keywords. All items in the search was exported as an xml file.

Script Description

    Read xml file with pubmed data
    Grab the abstract title, abstract text, and keywords from the xml
    Remove rows with any missing data or if they are duplicate publications (with corrections)
    Preprocess the text to remove any special characters
    Format keywords for running multilabel classification and remove 'alzheimers disease' as a keyword
    Pull the 20 most common keywords
    Remove keywords not listed in the 20 most common list
    Multilabel classification

Model Description

This is a classification problem since I want to classify each abstract with a set of keywords. There are many different types of classification algorithms, but a lot of them assume that only one label will be assigned to the sample. This particular problem would need multiple labels to be assigned, so the multilabel classification seemed like the best choice. With multilabel classification, you are able to assign multiple labels (or keywords) to an instance and there are no contraints on the amount of labels that can be assigned.

Steps needed to perform multilabel classification:

    Preprocess the keywords by first fitting the keywords to the MultiLabelBinarizer() estimator and later transforming the keywords (training and test set) into a binary matrix.
        The binary matrix format is needed to run further estimators.
    Split the data into a training set and a test set
    Build a vocabulary of words from the abstract text and count how many times each word is used. CountVectorizer estimator used to convert text into a matrix of counts, build a vocabulary of known words, and encode new text.
    Need to apply the term frequency-inverse document frequency (tf-idf) algorithm all of my abstrac texts.
        tf-idf is an algorithm for text mining that essentially outputs a weight for each word and evaluates how important the word is to all the documents. The importance of the word is measured by the number of times the words appear in the one document (term frequency), but is offset by how frequency of the word in all of the documents (inverse-document frequency). This alogrithm would avoid words such as and, the, I, etc.
    To predict multiple labels, we use the one-versus-rest algorithm (OneVsRestClassifier). This algorithm breaks down the problem into multiple independent binary classification.
    LinearSVC (Linear Support Vector Classification) is wrapped in the OneVsRestClassifier function. This classifier is used for linear classification of large datsets.
    Compute Hamming loss score: Evaluation for a multilabel classification is a bit different than in a single classification. Since there is a set of labels (multiple outputs), the set could be fully correct, partially correct, or completely incorrect. For these cases, it is useful to use the hamming loss measure. Hamming loss is the fraction of incorrect labels over the total number of labels. The optimal output value is zero.
    


