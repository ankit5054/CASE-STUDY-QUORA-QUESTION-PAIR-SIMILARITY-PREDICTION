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

### Preprocessing of Text
Preprocessing:
-  Removing html tags
-  Removing Punctuations
-  Performing stemming
-  Removing Stopwords
-  Expanding contractions etc.


### Advanced Feature Extraction 


Definition:

Token: You get a token by splitting sentence a space
Stop_Word : stop words as per NLTK.
Word : A token that is not a stop_word
Features:

cwc_min : Ratio of common_word_count to min lenghth of word count of Q1 and Q2 
cwc_min = common_word_count / (min(len(q1_words), len(q2_words)) 

cwc_max : Ratio of common_word_count to max lenghth of word count of Q1 and Q2 
cwc_max = common_word_count / (max(len(q1_words), len(q2_words)) 

csc_min : Ratio of common_stop_count to min lenghth of stop count of Q1 and Q2 
csc_min = common_stop_count / (min(len(q1_stops), len(q2_stops)) 

csc_max : Ratio of common_stop_count to max lenghth of stop count of Q1 and Q2
csc_max = common_stop_count / (max(len(q1_stops), len(q2_stops)) 

ctc_min : Ratio of common_token_count to min lenghth of token count of Q1 and Q2
ctc_min = common_token_count / (min(len(q1_tokens), len(q2_tokens)) 


ctc_max : Ratio of common_token_count to max lenghth of token count of Q1 and Q2
ctc_max = common_token_count / (max(len(q1_tokens), len(q2_tokens)) 


last_word_eq : Check if First word of both questions is equal or not
last_word_eq = int(q1_tokens[-1] == q2_tokens[-1]) 


first_word_eq : Check if First word of both questions is equal or not
first_word_eq = int(q1_tokens[0] == q2_tokens[0]) 


abs_len_diff : Abs. length difference
abs_len_diff = abs(len(q1_tokens) - len(q2_tokens)) 


mean_len : Average Token Length of both Questions
mean_len = (len(q1_tokens) + len(q2_tokens))/2 


fuzz_ratio : https://github.com/seatgeek/fuzzywuzzy#usage http://chairnerd.seatgeek.com/fuzzywuzzy-fuzzy-string-matching-in-python/ 


fuzz_partial_ratio : https://github.com/seatgeek/fuzzywuzzy#usage http://chairnerd.seatgeek.com/fuzzywuzzy-fuzzy-string-matching-in-python/ 


token_sort_ratio : https://github.com/seatgeek/fuzzywuzzy#usage http://chairnerd.seatgeek.com/fuzzywuzzy-fuzzy-string-matching-in-python/ 

token_set_ratio : https://github.com/seatgeek/fuzzywuzzy#usage http://chairnerd.seatgeek.com/fuzzywuzzy-fuzzy-string-matching-in-python/ 

longest_substr_ratio : Ratio of length longest common substring to min lenghth of token count of Q1 and Q2
longest_substr_ratio = len(longest common substring) / (min(len(q1_tokens), len(q2_tokens))


### Pair plot of features ['ctc_min', 'cwc_min', 'csc_min', 'token_sort_ratio']

![download](https://user-images.githubusercontent.com/39160589/60084102-ec5fd680-975c-11e9-9f0e-ae57625065de.png)

### Visualization
  Using TSNE for Dimentionality reduction for 15 Features(Generated after cleaning the data) to 3 dimention
  ![download](https://user-images.githubusercontent.com/39160589/60084297-406abb00-975d-11e9-9d78-1c86d027456e.png)

## MODELING WITH THE AVG W2V-TFIDF VECTORIZED DATA
### A Random Model (Finding worst-case log-loss)
Log loss on Test Data using Random Model 0.887242646958
![download](https://user-images.githubusercontent.com/39160589/60084792-31d0d380-975e-11e9-9a9e-2a6fccf32754.png)


### Logistic Regression
The test log loss is: 0.520035530431
![download](https://user-images.githubusercontent.com/39160589/60084918-6775bc80-975e-11e9-8b95-8aa6c94dc755.png)


### Linear SVM
The test log loss is: 0.489669093534
![download](https://user-images.githubusercontent.com/39160589/60084988-88d6a880-975e-11e9-9a08-fdcd92ead71c.png)


### XGBoost
The test log loss is: 0.357054433715
![download](https://user-images.githubusercontent.com/39160589/60085127-c20f1880-975e-11e9-882b-62df8800f21d.png)

## MODELING WITH THE TFIDF VECTORIZED DATA
### A Random Model (Finding worst-case log-loss)
Log loss on Test Data using Random Model 0.8940929603220081
![download](https://user-images.githubusercontent.com/39160589/60085401-3fd32400-975f-11e9-8fa3-6f326b9b3f20.png)


### Logistic Regression
The test log loss is: 0.6603882136027764 
![download](https://user-images.githubusercontent.com/39160589/60085912-2da5b580-9760-11e9-884f-a39e39843236.png)

### Linear SVM
The test log loss is: 0.6603882136027764
![download](https://user-images.githubusercontent.com/39160589/60085944-401fef00-9760-11e9-95e5-25184b91bf7a.png)

### XGBoost
The test log loss is: 0.357054433715
![download](https://user-images.githubusercontent.com/39160589/60086061-6f366080-9760-11e9-895a-e8468a6895c8.png)


