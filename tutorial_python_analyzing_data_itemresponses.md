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



### Pandas Crosstab

                                import numpy as np

                                from scipy.stats.stats import pearsonr
                                import itertools

                                from scipy.stats import chi2_contingency

                                import seaborn as sns
                                import matplotlib.pyplot as plts
                                
                                N = len(math_data_df.columns)
                                
                                contigency = [[] for i in range(N)]
                                for i in range(N):
                                    for j in range(N):
                                        if (i!=j and i<j):
                                            contigency[i].append(pd.crosstab(math_data_df[i], math_data_df[j], normalize=True))
                                            
                                  
Normalize Results for Question 10. Since we have over 200,000 responses the frequency we would have seen would be in the tens, hundred, thousands and ten thousands, and sometimes in the hundred thousands. So using a normalized value would help us see relatively how a certain cell compared to the others. There are various ways to normalize these values as they could be normalized only by rows or only by columns, but this is normalized throughout the entire table.  As you are looking through the tables you are reading how many of the responses from question 10 that were A selected either *, A, B, C, D, E, or _ for quetsion 11, 12 ,13 ....etc. It starts at question 11 since the way the loops were written above only compared the questions succeeding the question of interest. So Question 1 has 43 tables, Question 2 has 42 tables, and so on. This is because we don't need to do a table for Quetsion 2 and 1 again sine Question 1 already had a table with Question 2 and that would habe been redundant.
 
 
                                [11         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000004  0.000066  0.000075  0.000061  0.000162  0.000000  0.000022
                                 A   0.000092  0.011190  0.009226  0.008463  0.037150  0.000206  0.000649
                                 B   0.000394  0.065744  0.034722  0.027174  0.169028  0.000316  0.002546
                                 C   0.000285  0.052275  0.033074  0.022077  0.389337  0.000359  0.003463
                                 D   0.000088  0.021025  0.016037  0.011488  0.050268  0.000298  0.000916
                                 E   0.000004  0.000403  0.000421  0.000359  0.000600  0.000088  0.000048
                                 _   0.000070  0.004190  0.003292  0.002104  0.012838  0.000057  0.007245,
                                 12         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000004  0.000083  0.000061  0.000048  0.000171  0.000004  0.000018
                                 A   0.000075  0.026955  0.006250  0.005856  0.026740  0.000425  0.000675
                                 B   0.000158  0.106773  0.015375  0.018365  0.156270  0.000456  0.002529
                                 C   0.000281  0.333389  0.017896  0.019916  0.126597  0.000460  0.002332
                                 D   0.000127  0.039201  0.007635  0.009607  0.042028  0.000460  0.001061
                                 E   0.000000  0.000416  0.000237  0.000337  0.000666  0.000215  0.000053
                                 _   0.000022  0.009301  0.001249  0.001350  0.010927  0.000044  0.006903,
                                 13         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000048  0.000048  0.000096  0.000184  0.000000  0.000013
                                 A   0.000039  0.006890  0.005395  0.010765  0.042729  0.000206  0.000951
                                 B   0.000193  0.033043  0.012960  0.046823  0.202944  0.000513  0.003449
                                 C   0.000101  0.023177  0.011808  0.040143  0.421898  0.000451  0.003292
                                 D   0.000075  0.011365  0.006754  0.016213  0.064044  0.000285  0.001385
                                 E   0.000000  0.000294  0.000329  0.000443  0.000653  0.000153  0.000053
                                 _   0.000018  0.002270  0.001008  0.003362  0.015950  0.000070  0.007118,
                                 14         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000004  0.000039  0.000075  0.000197  0.000053  0.000004  0.000018
                                 A   0.000013  0.005246  0.009055  0.043483  0.007859  0.000329  0.000991
                                 B   0.000070  0.017484  0.038118  0.204986  0.033779  0.000723  0.004764
                                 C   0.000057  0.012724  0.026714  0.430839  0.026561  0.000552  0.003423
                                 D   0.000061  0.007337  0.013421  0.065302  0.012066  0.000451  0.001481
                                 E   0.000000  0.000263  0.000346  0.000732  0.000289  0.000237  0.000057
                                 _   0.000004  0.001214  0.002113  0.015507  0.002327  0.000066  0.008564,
                                 15         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000061  0.000131  0.000105  0.000070  0.000000  0.000022
                                 A   0.000013  0.008779  0.025969  0.021402  0.009252  0.000193  0.001367
                                 B   0.000053  0.035524  0.107877  0.102526  0.046424  0.000473  0.007048
                                 C   0.000026  0.029186  0.278028  0.141503  0.042699  0.000587  0.008840
                                 D   0.000035  0.012711  0.036782  0.033280  0.014902  0.000294  0.002117
                                 E   0.000000  0.000307  0.000600  0.000465  0.000298  0.000197  0.000057
                                 _   0.000009  0.002095  0.007477  0.006815  0.002735  0.000075  0.010589,
                                 16         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000149  0.000145  0.000061  0.000031  0.000000  0.000004
                                 A   0.000053  0.031417  0.024803  0.007249  0.002450  0.000153  0.000850
                                 B   0.000158  0.133737  0.126439  0.030527  0.005930  0.000162  0.002972
                                 C   0.000140  0.356307  0.110814  0.025610  0.005260  0.000215  0.002525
                                 D   0.000057  0.044110  0.039946  0.011282  0.003375  0.000158  0.001192
                                 E   0.000004  0.000482  0.000618  0.000355  0.000241  0.000167  0.000057
                                 _   0.000000  0.011225  0.009726  0.002314  0.000513  0.000018  0.006000,
                                 17         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000044  0.000140  0.000110  0.000070  0.000004  0.000022
                                 A   0.000053  0.009954  0.023554  0.019224  0.011873  0.000254  0.002064
                                 B   0.000197  0.047003  0.091796  0.097994  0.050987  0.000526  0.011422
                                 C   0.000136  0.051780  0.303537  0.087909  0.044066  0.000596  0.012846
                                 D   0.000066  0.015678  0.031395  0.030974  0.018132  0.000342  0.003533
                                 E   0.000000  0.000316  0.000451  0.000504  0.000302  0.000259  0.000092
                                 _   0.000009  0.002814  0.007008  0.004725  0.002612  0.000044  0.012583,
                                 18         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000096  0.000114  0.000096  0.000061  0.000004  0.000018
                                 A   0.000053  0.015765  0.031645  0.010475  0.007477  0.000140  0.001420
                                 B   0.000210  0.082614  0.129897  0.048769  0.032429  0.000386  0.005619
                                 C   0.000188  0.074024  0.352915  0.041945  0.027100  0.000259  0.004440
                                 D   0.000075  0.024374  0.045587  0.015971  0.011803  0.000267  0.002042
                                 E   0.000009  0.000473  0.000495  0.000346  0.000377  0.000153  0.000070
                                 _   0.000031  0.005842  0.010212  0.003344  0.002051  0.000057  0.008257,
                                 19         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000009  0.000079  0.000057  0.000180  0.000053  0.000000  0.000013
                                 A   0.000048  0.009778  0.008003  0.040389  0.006566  0.000311  0.001880
                                 B   0.000149  0.043185  0.031724  0.188844  0.027372  0.000456  0.008196
                                 C   0.000088  0.038035  0.026819  0.403051  0.024518  0.000425  0.007933
                                 D   0.000075  0.013657  0.011400  0.061151  0.010611  0.000263  0.002963
                                 E   0.000004  0.000316  0.000289  0.000662  0.000329  0.000245  0.000079
                                 _   0.000004  0.002481  0.001964  0.013898  0.001582  0.000035  0.009831,
                                 20         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000088  0.000136  0.000123  0.000031  0.000000  0.000013
                                 A   0.000075  0.007609  0.024974  0.028038  0.004427  0.000219  0.001635
                                 B   0.000180  0.031342  0.128824  0.116433  0.015595  0.000539  0.007013
                                 C   0.000153  0.029028  0.117682  0.333875  0.013188  0.000600  0.006342
                                 D   0.000070  0.011326  0.038618  0.040788  0.006421  0.000351  0.002546
                                 E   0.000013  0.000210  0.000592  0.000473  0.000245  0.000307  0.000083
                                 _   0.000013  0.002384  0.009550  0.009270  0.001218  0.000070  0.007289,
                                 21         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000004  0.000105  0.000136  0.000057  0.000066  0.000000  0.000022
                                 A   0.000048  0.023458  0.023365  0.009669  0.007390  0.000224  0.002823
                                 B   0.000197  0.089715  0.126062  0.043304  0.026670  0.000421  0.013556
                                 C   0.000171  0.261066  0.147293  0.041949  0.035892  0.000355  0.014144
                                 D   0.000092  0.032473  0.037281  0.015270  0.010541  0.000307  0.004155
                                 E   0.000009  0.000377  0.000500  0.000390  0.000259  0.000259  0.000131
                                 _   0.000013  0.005965  0.006491  0.002595  0.001604  0.000053  0.013074,
                                 22         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000149  0.000061  0.000075  0.000066  0.000004  0.000035
                                 A   0.000079  0.024715  0.005948  0.009888  0.023751  0.000184  0.002411
                                 B   0.000320  0.125567  0.019929  0.045968  0.097437  0.000416  0.010287
                                 C   0.000276  0.157593  0.020192  0.041187  0.270454  0.000438  0.010729
                                 D   0.000114  0.038263  0.008231  0.015143  0.034560  0.000210  0.003598
                                 E   0.000000  0.000451  0.000324  0.000394  0.000443  0.000193  0.000118
                                 _   0.000018  0.008779  0.001170  0.002980  0.007390  0.000061  0.009397,
                                 23         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000044  0.000066  0.000188  0.000066  0.000000  0.000026
                                 A   0.000044  0.007916  0.012001  0.037053  0.007394  0.000153  0.002415
                                 B   0.000158  0.033227  0.049957  0.172785  0.032298  0.000526  0.010975
                                 C   0.000136  0.030637  0.065924  0.364902  0.028529  0.000443  0.010300
                                 D   0.000061  0.012259  0.017150  0.054936  0.011575  0.000333  0.003804
                                 E   0.000004  0.000272  0.000456  0.000644  0.000241  0.000188  0.000118
                                 _   0.000013  0.002047  0.003278  0.012022  0.002099  0.000022  0.010313,
                                 24         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000009  0.000057  0.000131  0.000105  0.000061  0.000000  0.000026
                                 A   0.000061  0.010186  0.031785  0.012071  0.009480  0.000250  0.003143
                                 B   0.000171  0.040455  0.145558  0.054292  0.045122  0.000469  0.013859
                                 C   0.000158  0.052604  0.325175  0.064486  0.043071  0.000465  0.014911
                                 D   0.000075  0.014297  0.046963  0.018706  0.015082  0.000333  0.004663
                                 E   0.000000  0.000351  0.000469  0.000408  0.000342  0.000228  0.000127
                                 _   0.000009  0.002713  0.009384  0.003257  0.002639  0.000048  0.011746,
                                 25         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000004  0.000127  0.000092  0.000066  0.000079  0.000000  0.000022
                                 A   0.000118  0.032066  0.012553  0.012369  0.006368  0.000180  0.003322
                                 B   0.000267  0.145277  0.053814  0.059678  0.024978  0.000517  0.015393
                                 C   0.000276  0.337220  0.063347  0.058639  0.024496  0.000425  0.016467
                                 D   0.000101  0.046661  0.018728  0.019561  0.009559  0.000294  0.005216
                                 E   0.000004  0.000517  0.000390  0.000346  0.000276  0.000232  0.000158
                                 _   0.000026  0.010173  0.003458  0.003471  0.001407  0.000070  0.011190,
                                 26         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000013  0.000118  0.000057  0.000101  0.000070  0.000000  0.000031
                                 A   0.000092  0.012654  0.010848  0.026179  0.012211  0.000254  0.004738
                                 B   0.000276  0.061537  0.048186  0.109043  0.056159  0.000693  0.024032
                                 C   0.000193  0.057154  0.050527  0.294188  0.068935  0.000609  0.029265
                                 D   0.000110  0.019780  0.016660  0.035585  0.020087  0.000333  0.007565
                                 E   0.000000  0.000298  0.000337  0.000443  0.000399  0.000241  0.000206
                                 _   0.000018  0.002739  0.001959  0.005435  0.002608  0.000079  0.016958,
                                 27         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000004  0.000070  0.000101  0.000105  0.000079  0.000000  0.000031
                                 A   0.000079  0.022581  0.007398  0.020341  0.012978  0.000254  0.003344
                                 B   0.000224  0.113268  0.027306  0.071004  0.073099  0.000399  0.014626
                                 C   0.000250  0.157808  0.031036  0.246624  0.051289  0.000390  0.013473
                                 D   0.000123  0.036869  0.010502  0.025811  0.021547  0.000329  0.004940
                                 E   0.000004  0.000403  0.000316  0.000434  0.000377  0.000197  0.000193
                                 _   0.000031  0.008770  0.001762  0.005755  0.004606  0.000022  0.008849,
                                 28         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000061  0.000092  0.000079  0.000114  0.000004  0.000039
                                 A   0.000039  0.008744  0.015656  0.013793  0.024128  0.000210  0.004405
                                 B   0.000167  0.037465  0.078196  0.065056  0.098099  0.000473  0.020468
                                 C   0.000184  0.035156  0.083048  0.063299  0.295389  0.000465  0.023330
                                 D   0.000061  0.012575  0.025377  0.021310  0.033665  0.000447  0.006684
                                 E   0.000000  0.000351  0.000443  0.000377  0.000399  0.000188  0.000167
                                 _   0.000018  0.002034  0.004067  0.003677  0.006772  0.000096  0.013131,
                                 29         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000004  0.000088  0.000053  0.000153  0.000057  0.000000  0.000035
                                 A   0.000048  0.010221  0.006811  0.039447  0.006360  0.000316  0.003774
                                 B   0.000110  0.047954  0.026784  0.183475  0.023922  0.000675  0.017006
                                 C   0.000127  0.033420  0.023791  0.403832  0.023650  0.000469  0.015581
                                 D   0.000039  0.015305  0.009647  0.059428  0.009717  0.000333  0.005650
                                 E   0.000004  0.000342  0.000254  0.000622  0.000329  0.000206  0.000167
                                 _   0.000004  0.003366  0.001503  0.014477  0.001652  0.000061  0.008731,
                                 30         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000004  0.000101  0.000136  0.000053  0.000048  0.000000  0.000048
                                 A   0.000013  0.019395  0.027012  0.009182  0.006588  0.000175  0.004611
                                 B   0.000088  0.093269  0.112532  0.042611  0.029431  0.000421  0.021573
                                 C   0.000075  0.125905  0.284024  0.039845  0.029966  0.000329  0.020727
                                 D   0.000026  0.029230  0.038487  0.014744  0.010445  0.000250  0.006938
                                 E   0.000000  0.000412  0.000465  0.000377  0.000245  0.000245  0.000180
                                 _   0.000013  0.006373  0.007394  0.002397  0.001727  0.000044  0.011847,
                                 31         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000070  0.000105  0.000110  0.000057  0.000000  0.000048
                                 A   0.000044  0.015126  0.010778  0.021152  0.014420  0.000153  0.005303
                                 B   0.000162  0.060756  0.042940  0.107597  0.063360  0.000355  0.024755
                                 C   0.000184  0.145391  0.071433  0.149261  0.102635  0.000386  0.031579
                                 D   0.000105  0.020631  0.015695  0.033972  0.021547  0.000210  0.007959
                                 E   0.000004  0.000311  0.000399  0.000543  0.000329  0.000153  0.000184
                                 _   0.000009  0.003734  0.002621  0.005825  0.003559  0.000044  0.014004,
                                 32         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000088  0.000131  0.000057  0.000061  0.000000  0.000053
                                 A   0.000061  0.016804  0.026578  0.010655  0.007289  0.000136  0.005452
                                 B   0.000254  0.058070  0.143059  0.043667  0.028901  0.000316  0.025658
                                 C   0.000197  0.224604  0.140828  0.054164  0.052630  0.000311  0.028134
                                 D   0.000096  0.021244  0.043597  0.015945  0.010900  0.000197  0.008139
                                 E   0.000018  0.000342  0.000565  0.000324  0.000302  0.000158  0.000215
                                 _   0.000026  0.004120  0.008547  0.002809  0.002218  0.000022  0.012053,
                                 33         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000088  0.000057  0.000114  0.000088  0.000000  0.000044
                                 A   0.000088  0.013653  0.009108  0.026342  0.011470  0.000254  0.006062
                                 B   0.000294  0.058845  0.039438  0.121320  0.050996  0.000403  0.028629
                                 C   0.000237  0.081413  0.039267  0.279395  0.067423  0.000377  0.032758
                                 D   0.000136  0.019110  0.013916  0.039328  0.018180  0.000254  0.009195
                                 E   0.000004  0.000281  0.000377  0.000425  0.000373  0.000224  0.000241
                                 _   0.000004  0.003831  0.002314  0.007876  0.003116  0.000057  0.012597,
                                 34         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000053  0.000118  0.000114  0.000048  0.000004  0.000053
                                 A   0.000066  0.008884  0.024369  0.020315  0.007083  0.000237  0.006022
                                 B   0.000281  0.041862  0.103411  0.096394  0.028288  0.000465  0.029225
                                 C   0.000259  0.037601  0.252686  0.154381  0.024746  0.000329  0.030869
                                 D   0.000110  0.013982  0.034402  0.031115  0.010905  0.000272  0.009336
                                 E   0.000004  0.000316  0.000390  0.000451  0.000329  0.000184  0.000250
                                 _   0.000035  0.002713  0.007539  0.006105  0.001727  0.000057  0.011619,
                                 35         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000066  0.000066  0.000070  0.000123  0.000000  0.000066
                                 A   0.000083  0.009936  0.011501  0.011247  0.026521  0.000228  0.007460
                                 B   0.000333  0.038399  0.053880  0.051096  0.118348  0.000587  0.037281
                                 C   0.000316  0.043869  0.055940  0.050728  0.304483  0.000539  0.044995
                                 D   0.000131  0.013824  0.018115  0.017786  0.038377  0.000333  0.011553
                                 E   0.000013  0.000289  0.000316  0.000399  0.000425  0.000202  0.000281
                                 _   0.000018  0.001898  0.002538  0.002336  0.006320  0.000066  0.016620,
                                 36         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000044  0.000127  0.000053  0.000101  0.000000  0.000066
                                 A   0.000088  0.011812  0.024040  0.007959  0.015406  0.000237  0.007433
                                 B   0.000272  0.049878  0.106712  0.032188  0.073616  0.000565  0.036694
                                 C   0.000259  0.073274  0.260106  0.035502  0.085485  0.000513  0.045732
                                 D   0.000153  0.017536  0.034178  0.012263  0.024010  0.000320  0.011659
                                 E   0.000009  0.000302  0.000425  0.000324  0.000346  0.000219  0.000298
                                 _   0.000031  0.002958  0.006408  0.001525  0.003818  0.000039  0.015016,
                                 37         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000004  0.000075  0.000105  0.000088  0.000053  0.000000  0.000066
                                 A   0.000101  0.011803  0.017887  0.017098  0.011374  0.000259  0.008455
                                 B   0.000394  0.052508  0.067077  0.084004  0.051903  0.000767  0.043273
                                 C   0.000281  0.079949  0.199297  0.099116  0.063645  0.000710  0.057872
                                 D   0.000140  0.017878  0.023475  0.027279  0.017729  0.000329  0.013289
                                 E   0.000013  0.000272  0.000333  0.000373  0.000373  0.000263  0.000298
                                 _   0.000018  0.002454  0.003673  0.003375  0.002354  0.000070  0.017852,
                                 38         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000101  0.000088  0.000066  0.000070  0.000000  0.000066
                                 A   0.000083  0.024746  0.015551  0.009038  0.009770  0.000140  0.007648
                                 B   0.000241  0.103692  0.072612  0.036256  0.049532  0.000381  0.037211
                                 C   0.000285  0.260211  0.097599  0.045403  0.050259  0.000381  0.046731
                                 D   0.000167  0.034253  0.024036  0.013745  0.015875  0.000289  0.011755
                                 E   0.000009  0.000386  0.000421  0.000324  0.000346  0.000140  0.000298
                                 _   0.000022  0.007227  0.004313  0.002086  0.002849  0.000039  0.013258,
                                 39         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000075  0.000079  0.000083  0.000083  0.000004  0.000066
                                 A   0.000114  0.011225  0.021796  0.010550  0.014933  0.000281  0.008078
                                 B   0.000337  0.044241  0.086778  0.045780  0.082316  0.000561  0.039911
                                 C   0.000337  0.069724  0.243460  0.049597  0.086322  0.000465  0.050965
                                 D   0.000149  0.016331  0.030575  0.016138  0.024172  0.000241  0.012513
                                 E   0.000009  0.000294  0.000425  0.000316  0.000373  0.000193  0.000316
                                 _   0.000009  0.002748  0.005413  0.002656  0.004497  0.000092  0.014380,
                                 40         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000088  0.000057  0.000092  0.000079  0.000004  0.000070
                                 A   0.000075  0.013127  0.018334  0.016927  0.009498  0.000188  0.008827
                                 B   0.000316  0.057097  0.090008  0.069755  0.039302  0.000478  0.042970
                                 C   0.000316  0.095662  0.117051  0.174582  0.056952  0.000447  0.055861
                                 D   0.000101  0.018991  0.029554  0.022932  0.014293  0.000241  0.014008
                                 E   0.000018  0.000377  0.000390  0.000337  0.000311  0.000175  0.000316
                                 _   0.000013  0.003357  0.005251  0.003879  0.002174  0.000053  0.015069,
                                 41         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000149  0.000083  0.000057  0.000031  0.000000  0.000070
                                 A   0.000101  0.026249  0.012242  0.014091  0.005597  0.000289  0.008406
                                 B   0.000346  0.140557  0.049356  0.047358  0.021433  0.000373  0.040503
                                 C   0.000272  0.185872  0.088176  0.150090  0.024194  0.000316  0.051951
                                 D   0.000153  0.042379  0.017383  0.018115  0.008485  0.000259  0.013346
                                 E   0.000004  0.000478  0.000320  0.000346  0.000250  0.000188  0.000337
                                 _   0.000009  0.009848  0.003051  0.003020  0.001183  0.000039  0.012645,
                                 42         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000004  0.000079  0.000101  0.000066  0.000066  0.000000  0.000075
                                 A   0.000083  0.009147  0.023295  0.011545  0.014004  0.000162  0.008740
                                 B   0.000289  0.043378  0.116205  0.047695  0.050483  0.000473  0.041401
                                 C   0.000329  0.040498  0.131716  0.086576  0.186380  0.000513  0.054857
                                 D   0.000101  0.014350  0.036133  0.017124  0.018448  0.000237  0.013727
                                 E   0.000004  0.000263  0.000438  0.000373  0.000311  0.000206  0.000329
                                 _   0.000026  0.002551  0.007618  0.003046  0.003809  0.000061  0.012684,
                                 43         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000000  0.000053  0.000101  0.000105  0.000048  0.000000  0.000083
                                 A   0.000039  0.008814  0.015349  0.021639  0.010747  0.000245  0.010142
                                 B   0.000224  0.036168  0.071560  0.095737  0.045320  0.000522  0.050395
                                 C   0.000224  0.043935  0.106869  0.222272  0.053687  0.000600  0.073283
                                 D   0.000092  0.012610  0.022870  0.031228  0.016598  0.000311  0.016410
                                 E   0.000000  0.000215  0.000447  0.000381  0.000351  0.000197  0.000333
                                 _   0.000004  0.001907  0.003204  0.004142  0.002130  0.000061  0.018347,
                                 44         *         A         B         C         D         E         _
                                 10                                                                      
                                 *   0.000004  0.000061  0.000092  0.000066  0.000079  0.000009  0.000079
                                 A   0.000004  0.014306  0.017782  0.012417  0.012732  0.000316  0.009419
                                 B   0.000070  0.065337  0.086537  0.054984  0.046578  0.000666  0.045754
                                 C   0.000105  0.088706  0.135670  0.080230  0.133377  0.000535  0.062247
                                 D   0.000022  0.020942  0.027740  0.019035  0.016896  0.000399  0.015086
                                 E   0.000000  0.000412  0.000324  0.000364  0.000245  0.000219  0.000359
                                 _   0.000013  0.003997  0.005553  0.003314  0.002796  0.000057  0.014065]

Now lets generate a visualization of these tables we created. The benefit of Python and Matplotlib is to generate visualization plots that can help use interpret and analyze our results without combing through an endless series of tables like we have above. So lets prepare our data for Matplotlib by putting the tables into a list.

We have over 300 tables in contigency_pct. So lets just plot one set for Question 1. 

                                filtered_crosstab_pct_10=[]
                                
                                for i,x in enumerate(contigency_pct):
                                    for j,y in enumerate(x):
                                        label = list(y.columns)
                                        for k in label:
                                            z = y[k]
                                            
                                            filtered_crosstab_pct05.append(y)
                                
                                plts.figure(figsize=(10,6))

                                def heatmap2d(arr: np.ndarray):
                                    plts.imshow(arr, cmap='viridis')
                                    plts.colorbar()
                                    plts.show()


                                for x in filtered_crosstab_pct_10:
                                    heatmap2d(x)
                                    
![Heat Map](/heat_map_crosstab_math_data.png)                                

From what you see above is for Question 1 and 2, the most common sequence of responses was selecting C for Question 1 and then selecting B for Question 2. From our table we see that there was nearly 57% of the over 200,000 responses gave this sequence. The next highest frequency was selection B for Question 1 and then B for Question 2.

