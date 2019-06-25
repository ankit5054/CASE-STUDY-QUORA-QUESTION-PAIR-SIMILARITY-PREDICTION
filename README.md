### PROBLEM STATEMENT

![sl26](https://user-images.githubusercontent.com/39160589/60081650-35f9f280-9758-11e9-97a4-91af2c054fd7.jpg)

- Identify which questions asked on Quora are duplicates of questions that have already been asked.
- This could be useful to instantly provide answers to questions that have already been answered.
- We are tasked with predicting whether a pair of questions are duplicates or not.

### Business Objectives and Constraints

- The cost of a mis-classification can be very high.
- You would want a probability of a pair of questions to be duplicates so that you can choose any threshold of choice.
- No strict latency concerns.
- Interpretability is partially important.

### Data Overview
-To download dataset https://www.kaggle.com/c/quora-question-pairs 
- Data will be in a file Train.csv 
- Train.csv contains 5 columns : qid1, qid2, question1, question2, is_duplicate 
- Size of Train.csv - 60MB 
- Number of rows in Train.csv = 404,290

        id: Looks like a simple rowID
        qid{1, 2}: The unique ID of each question in the pair
        question{1, 2}: The actual textual contents of the questions.
        is_duplicate: The label that we are trying to predict - whether the two questions are duplicates of each other.

### To Achieve
It is a binary classification problem, for a given pair of questions we need to predict if they are duplicate or not.

### Performance Metric

- log-loss : https://www.kaggle.com/wiki/LogarithmicLoss
- Binary Confusion Matrix

### Distribution of data points among output classes
- Question pairs are not Similar (is_duplicate = 0):
   63.08%

- Question pairs are Similar (is_duplicate = 1):
   36.92%
   
   ![download](https://user-images.githubusercontent.com/39160589/60083694-30061080-975c-11e9-8fb6-5b754cbced25.png)
