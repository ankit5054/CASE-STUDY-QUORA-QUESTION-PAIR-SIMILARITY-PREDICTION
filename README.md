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
