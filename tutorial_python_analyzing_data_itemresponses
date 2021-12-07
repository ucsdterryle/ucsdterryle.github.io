# Tutorial on Analyzing Data from Large Dataset

This tutorial will be a series of exercises aimed at parsing out data taken from a CSV file and imported in to a Pandas DataFrame. There is no defined goal in this project or results we wish to report. However, we will have the intention of using the data found in the dataset to uncover possible patterns through various techniques and tools found in various modules in the Python libraries. The goal of this tutorial is to become more familiar and fluent in using various libraries like Pandas, Matplotlib, NetworkX, and Numpy, as well as working with various data structures and the syntax to manipulate and handle them properly.

### About the Dataset
The dataset we are working with is a comprehensive collection of responses from a mathematics diagnostic test/assessment. The diagnostic test has 45 questions (columns) and over 200,000 submissions were collected. No solution key is provided. No identifiers of any kind is present regarding the test takers. Each quetsion was multiple choice (A,B,C,D,E) and students had the option of omitting/skipping questions. There are some instances where ``*'' was present in the CSV file. 

### Getting Started

To begin this exercise we need to first import the data housed in a CSV (commma separated value) file into a structure that we can handle and work with in Python. This is best served with Pandas DataFrame. Essentially, it is a robust table we can work with throughout this tutorial. It will take some time to get used to as there are very nuance details that will often confuse beginners when working with DataFrames. But once you get the handle for how to select and filter data found in the DataFrame, you will be better able to anticipate potential errors and how to handle them. There is a lot of functions that make working with data in a DataFrame the optimal choice in analyzing data in Python. Initially, it can be overwhelming, however, with practice you can start to get more of an intuitive feel of working with a DataFrame and adopting more of the functions into your routines when cleaning and analyzing your data.

Lets import the data into our DataFrame by first opening the CSV file and dumping all of the information into our Pandas DataFrame. It is always good to take a look at the data first in a program like MS Excel to get an idea of the general layout and attributes of the data. If you were to open the file, you would see that there is not an explicit label to the columns that identifies the question. So we will assume that the first column is Question 1 and sequentially goes up until Question 45. 

        import pandas as pd
        
        math_data_df = pd.read_csv("AR45A00Responses.csv", header=None)

It is as simple as that. Essentially one line to fulfill that goal of importing the data. Since there is no column label in the CSV file for the names of the columns we include the parameter 'header=None' to let Pandas know not to treat the first row as labels for the column, but instead include the first row as data. If we had not included the 'header=None' parameter, the DataFrame would have labeled the first column 'B' since that was the first data found on the first row.

### Understanding the Dataset
Despite mentioning earlier that we should look at the data by openning it in MS Excel, we will continue to understand the data provided to us using various functions to look at the attributes of our DataFrame now that all the data has been imported from the CSV file.

        math_data_df.shape
        
        (228160, 45)
        
Using the shape attribute will return the dimensions of the DataFrame. In our DataFrame, we have 228,160 rows and 45 columns.  

        math_data_df.head()
        
To look at the data in the DataFrame, we have to constantly use the variable name `math_data_df' whenever you want to access the DataFrame. The function `head()' is a DataFrame function that will take the first 5 rows of the DataFrame and print it to screen. This simple function helps to easily get an overview of the data found in the beginning of the DataFrame and to get an idea of the kind of entries/values found and look at the labels of the columns if they exist. If we were to insert an integer value as a parameter for the head() function, it will return the first N rows of the DataFrame.

        math_data_df.head(12)
        

 	0 	1 	2 	3 	4 	5 	6 	7 	8 	9 	... 	35 	36 	37 	38 	39 	40 	41 	42 	43 	44
0 	B 	B 	A 	C 	A 	D 	B 	C 	D 	B 	... 	B 	C 	C 	A 	D 	C 	B 	D 	C 	D
1 	C 	B 	D 	A 	C 	A 	C 	B 	C 	B 	... 	C 	B 	D 	C 	B 	C 	A 	D 	B 	C
2 	D 	C 	A 	A 	C 	C 	B 	B 	B 	C 	... 	B 	C 	C 	B 	C 	A 	C 	B 	B 	B
3 	B 	D 	C 	C 	A 	B 	C 	B 	B 	C 	... 	C 	D 	A 	B 	C 	B 	C 	B 	C 	B
4 	B 	B 	C 	A 	A 	D 	B 	C 	C 	B 	... 	B 	D 	D 	D 	D 	A 	A 	B 	A 	B
5 	D 	B 	D 	B 	A 	C 	B 	D 	A 	C 	... 	B 	D 	C 	A 	B 	D 	A 	B 	_ 	D
6 	A 	B 	C 	A 	C 	C 	B 	A 	A 	A 	... 	A 	C 	C 	A 	B 	D 	A 	B 	A 	B
7 	C 	B 	B 	A 	B 	C 	B 	D 	C 	A 	... 	D 	D 	B 	B 	B 	C 	A 	C 	C 	D
8 	B 	B 	D 	A 	A 	D 	B 	D 	B 	A 	... 	B 	D 	_ 	C 	E 	_ 	D 	A 	D 	D
9 	B 	B 	A 	A 	B 	D 	B 	D 	B 	D 	... 	B 	D 	D 	A 	C 	B 	A 	C 	B 	C
10 	B 	B 	A 	A 	A 	B 	B 	B 	C 	C 	... 	_ 	D 	A 	C 	B 	C 	A 	B 	D 	B
11 	C 	B 	B 	A 	D 	C 	B 	D 	D 	D 	... 	C 	B 	A 	A 	C 	D 	A 	C 	D 	B

12 rows × 45 columns

The above line of code will provide a snippet of the DataFrame from row 0 to row 11.

### Cleaning the Dataset

A required step whenever working with data is to clean the data. We cannot expect that the data that we recieved is entirely valid and complete. It is common to find NaN in your dataset. `NaN' means `not a number' which is not handled well as you are reading and processing it in your code. It is essential to identify and replace these with actual values so it does not cause errors in your code or to eliminate any rows that contain `NaN.' To do this we simply use this function 

        math_data_df[i].fillna(0)

This simple function easily replaces an instance of `NaN' in our DataFrame into a 0 integer. However, lets explore our data more. So lets assume we didn't replace our `NaN' entries with 0's just yet. Suppose we might have not known that this was a multiple choice test or we were just curious about what other possible entries we would find in each column. We would simply use the unqiue function.

        math_data_df[1].unique()
        
        array(['B', 'C', 'D', '_', 'A', 'E', '*', nan], dtype=object)
        
What the above line of code does is return an array of all the unique entries found in the second column. What we see is that we have not only, 'A,B,C,D,E' but also '_', '*', and our `NaN' in this column. This is an incredibly useful function since our dataset was over 200,000 rows it would be tedious to search through all the rows. In addition, if the range of our data was `wider' it would not be as easy to find every different entry out of 200,000 rows.    


### Counting
In this tutorial the challenge begins when we noticed we have over 200,000 rows to work with. One idea we would like to pursue in analyzing our dataset is to see how many A's, B's, C's, ... etc. are found in each question (column). We can write a loop that will iterate through each column and at each column it will iterate through all 200,000+ rows and count the string it finds. We will use a dictionary data structure in Python to hold this information counting the occurences of each response choice. 


        response_dict={}
        for i in math_data_df:
            x = math_data_df[i].fillna(0)
            count = {'A': 0, 'B': 0, 'C': 0, 'D': 0, 'E': 0, '_': 0, '*': 0, 0: 0}
            for j in x:
                count[j]+=1
            response_dict[i]=count

        response_dict

Notice that there are two nested for-loops the first that iterates through the columns `for i in math_data_df' and then the next for-loop iterates through each row of that column used designated `i'. In addition, the dictionary we are using is a nested dictionary. So for each column/question there is another dictionary for the entries and for the entire diagnostic test it is a dictionary where the keys are the question number and the value is the dictionary of responses. Everytime the second loop is completed the dictionary merely takes the count dictionary for that question. 

However, since this is such a common need when working with large data sets there is a Pandas DataFrame function that will perform the same function as our code above. 

        N = len(math_data_df.columns)
        for q in range(N):
            response_count[q] = math_data_df[q].value_counts().to_dict()
            
The first line merely gives us an integer for us to properly iterate through the entire DataFrame's columns. Then we use a for-loop with `range' and use the function `value_counts() which returns the count of all the unique entries. Then we also use the to_dict() function that returns what is given by `value_counts()' in a dictionary data structure. The reason for writing the code as I did above is that now I can access each question as a key in the `response_count' dictionary. So if I wanted to see the distribution of responses for question 7, I can simply use:

        response_count[6]
        
        {'B': 165329, 'D': 32192, 'A': 15975, 'C': 9489, '_': 4359, 'E': 719, '*': 94}
        
Notice that the dictionary is `ordered' such that the first key has the highest frequency and the last key has the lowest frequency. We will use this in our next line of code. 
        
Since we do not have a solution key, we will assume that for each question the choice with the greatest frequency is the `correct' solution. Since the first key in our dictionary has the highest frequency count we merely have to use the first key and value for every question to get our solution. 
