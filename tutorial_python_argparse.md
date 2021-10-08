# Tutorial on Using Argparse Module

As easy and simple as Python make it to run functions and scripts there will be a time that you may want to interact with your Python code through a terminal instead of running your code through a Jupyter Notebook cell. Even though I had been running programs through the terminal in my other courses that used Bash, C/C++, or Java, I had never run a Python script through a terminal. In fact I found it very inefficient thinking about and imagining running a Python script in a terminal. However, I couldn't be more worng. Writing your python script/program to execute through a terminal can be very efficient and I felt was a natural progression in my ability to code Python such that it made me more comfortalbe working with ohter programs that required execution from the terminal.

### Why would we want to run code fromt he terminal?
There are many reasons to start writing your Python code to run through the terminal. But, namely, the biggest reason I would say is that it allows you to start writing code such that one file can be used to run a number of different ways by just indicating that custom options when running your code from the terminal. Yes, there will be some extra front end cost to writing your code to look for options in the command line and planning out your code ahead of time to decide how to process the parameters that can be changed.

For this tutorial we will only need to import two modules: sys and argparse

Instead of running this python script/code/file in a Jupyter Notebook, we will be using and IDE like PyCharm. 

# Example 1: Reading a file

The characters/strings that follow the python file we wish to run can be thought of as 'options' at this point. In actuality, they are merely characters and strings that our 'argparse' module will scan and read. Depending on how you write your script, these strings can be ignored or used as parameters for other parts of your code to use. Our first example in using these 'options' is to provide the filename that we want to have our Python script open and read from. This is often seen used in a Jupyter Notebook as:

with open('filename.txt', 'r') as f:

or 

file = open('filename.txt', 'r'):

Instead we can provide the filename on the command line and have our script take that string as an asignment that can be accessed later in time.

import sys
import argparse

def tutorial_options():
  parse = argparse.ArgumentParser()
  parser.add_argument('-r', type=str, help='read input filename', required=True)
  args = parser.parse_args()
  return args
    
def main():
    args = dbOptions()
    filename = args.w
    type = args.t
    if(type=='n'):
        type = 'nucleotide'
    else:
        type = 'protein'
    readfile = args.r
    queryList = file2List(readfile)
    returnDict = dict()
    # queryList , run a function to generate list
    returnDict = getData(type, filename, queryList)

