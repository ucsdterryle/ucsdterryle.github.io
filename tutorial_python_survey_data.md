# Surveying and Plotting Data in Python

I remember the first time I was learning Python while doing an assignment in my (graduate) physics course. The only thing people had told me was that Python was easy and that I can Google and YouTube anything I needed to figure out.  However, much of my attempts at Google-ing my questions still did not resolve my confusion, so hopefully I will go into lengths to describe some of my issues and some tangents that might help anyone that was as clueless as me at the time learn how to perform specific tasks throughout this tutorial.

In this tutorial we will working with a dataset that was downloaded from Kaggle that contains a list of movies and tv shows on Netflix and some metadata about those movies and tv shows. The file is a .csv (comma-separated-value) file that is often used like a spreadsheet document that organizes information in a table like format. We will need to first download this file to a local directory so we can access and read the file. We will then take the information in the file and do some basic exploration and use a visualization tool to give us an idea of what is in this file.

Looking at our data is often considered the first step as a data scientist. Often times the challenge is that the file/dataset is very large and going through the data by inspection can be overwhelming. So we enlist the help of Python and various data science tools to help us organize the data we are working with. So lets get started!

1. In this tutorial I will be using Jupyter Notebook, but you are welcome to use any Python IDE of your choosing. (What is Jupyter Notebook)
2. Download the dataset from Kaggle from this [link](https://www.kaggle.com/niharika41298/netflix-visualizations-recommendation-eda)
3. Install matplotlib and pandas libraries if your current IDE does not have them.


## Reading the downloaded CSV file
How do we have python open and read this file we downloaded with the data we can to look at? There are many ways of doing this! There are ways to read a file without the help of other libraries by just using native (built-in) functions in Python. However, for this tutorial we will use Pandas to help us import the data since this will give us a quick introduction to this very helpful library often used in data science.

    import pandas as pd
  
    netflix_titles = pd.read_csv('netflix_titles.csv')
    netflix_titles.head(5)

netflix_titles is a variable that will allow us to view the data on the csv file in a DataFrame object. Simply, for now, you can think of a DataFrame as a very fancy (Excel) spreadsheet.

The head() function allows you to take a peak at the first few rows of the DataFrame. If you wanted to look at 10 rows instead of 5 you can just replace the 5 with 10. However, probably the most important information we want to get from our file are all the different columns and the type/kind of information it has.  While we can tell from the name of the file that this is probably a file containing a list of titles of movies and tv shows on Netflix, we don't know what information about those movies and tv shows this file could have for us.

    col_labels1 = movies.column
    col_labels1
  
Another way we could have accessed this information is by using the values attribute (not function) instead of the columns attribute of the DataFrame.

    col_labels2 = movies.columns.values
    col_labels2

Another way we could have access this information is by using a for-loop that would go through each of the columns in the DataFrame and print out the labels/header of each column.

    for columns in netflix_titles:
      print(columns)
    
Note that while each one of these approaches gave us the information we wanted, we should probably use the first two approaches rather than the third. The third approach only prints out to the screen the labels on the columns but once its done printing them to the screen all the information printed isn't accessible again unless we run the for-loop. However, in the first two approaches what we did was save the actual labels as string in a list/array data structure. All we need to do if we ever wanted those labels, we just use those variables we created to hold the information.

Now, lets continue exploring this file. As I mentioned earlier often times when we work with data sets, the amount of data we are working with can be very high. So how would we go about understanding how many titles this file has? There is an attribute called 'size'

    netflix_titles.size
  
    93444
  
What we get is a number '93444.' However, this number is rather ambiguous. What I should have used is the 'shape' attribute.

    netflix_titles.shape
  
    (7787, 12)
  
What we get now is the dimensions of our spreadsheet/table/DataFrame. Piecing the information together from what we got earlier regarding the column labels, we have a DataFrame with 12 columns and 7787 rows. This tells us that our data set has 7787 titles with 12 columns of information about that TV Show or Movie. In total there are 93444 cells in our DataFrame since 7787 x 12 = 93444.

Our dataset has nearly 8000 TV shows and movies! So it might be prudent of us to look at how many of those 8000 are TV show and how many are movies. So lets start exploring the composition of our data set.

Movies to TV Shows: There is a column labelled 'type' that we can use to differentiate between 'TV Show' and 'Movies'. There are a number of ways to access this column on the DataFrame. 

1. We can access the column 'type' by referencing the string using brackets on the dataframe object. 
    
        movies['type']

2. We can acces the column 'type' by referencing the integer index of the column in the order it appears in the DataFrame. In our dataset 'type' is the second column so it's index is 1. The parameters of iloc is to indicate the rows to select and the columns. We will use the colon to indicate all rows and  only 2nd column. 
  
        movies.iloc[:,1]

3. We can access the column 'type' by referencing the string label using the loc function. 

        movies.loc[:,['type']]
      
Getting familiar with the loc and iloc DataFrame functions still is something that confuses me but after some practice you'll start to get the handle of it. But for now the easiest approach might be to use the first example using the label of the column in brackets.  

Now that we know how to access the specific columns, we need to organize the data within that column. There are numerous ways to approach this task depending on our end goal and how we wish to use the information we sort.  But for now we will explore the the various ways to go about doing this for the sake of education purposes.

Note: that 'type' is a categorical data not numerical.

1. Iterate with a Condition:

  Now that we have accessed the column data, we can iterate through the data using a for-loop and since we know the data consist of two different categories we can merely count the instances when "movies" or "TV show" appears.
  
    movie_count=0
    tv_show_count=0
    for x in movies['type']:
      if(x=='Movie'):
        movie_count+=1
      else:
        tv_show_count+=1

    print("Number of TV Shows: ", tv_show_count)
    print("Number of Movies: ", movie_count)
    
    Number of TV Shows:  2410
    Number of Movies:  5377

2. Sort/Select with DataFrame and Count:
   
        movie_count=0
        tv_show_count=0
        for x in movies.iloc[:,1]:
          if(x=='Movie'):
            movie_count+=1
          else:
            tv_show_count+=1

        print("Number of TV Shows: ", tv_show_count)
        print("Number of Movies: ", movie_count)
        
        Number of TV Shows:  2410
        Number of Movies:  5377


Now that we have counted our data, lets create a few visualizations with Matplotlib module to understand our data a bit more.

1. Pie Chart

In this example of generating a pie chart with Matplotlib we will use simple variables that are integers that the Matplotlib module will use to generate the proportions for the pie chart. Since this out first time using matplotlib in our code, we need to import the module/library. 

    import matplotlib.pyplot as plt

    pie_list = [tv_show_count, movie_count]
    pie_tuple = (tv_show_count, movie_count)
    plt.pie(pie_tuple)
    plt.show()

    ![pie_chart_netflix_titles](/assets/images/tux.png)

2. Bar Chart