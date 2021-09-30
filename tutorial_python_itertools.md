# Using Itertools in Python

A very common a basic intersection of programming and mathematics (combinatorics) might come about when you might need to generate all possible combination/permutations from a single or collection of sets. Often times in introductory probability and discrete math courses you will encounter this problem without needing to use a computer to assist you but rather to understand that concept you'll work through this problem analytically. 

Suppose you have a "group" of 5 objects: 'A', 'B', 'C', 'D', E'. We want to make every possible pair without repeating objects (no {'A', 'A'}). With only 5 objects this can be achieved by hand. But lets try to use computers/programming (with Python) to try to solve this problem.

This first approach is somewhat a naive or direct approach I believe we all will probably fall back to. It isn't egregiously wrong, but it might not be the best approach, even though it does achieve the goal to a certain extent.

        group = ['A', 'B', 'C', 'D', 'E']
        pairs = []
        for x in group:
          for y in group:
            if (x!=y):
              pairs.append((x,y))
        pairs
        
        [('A', 'B'),
         ('A', 'C'),
         ('A', 'D'),
         ('A', 'E'),
         ('B', 'A'),
         ('B', 'C'),
         ('B', 'D'),
         ('B', 'E'),
         ('C', 'A'),
         ('C', 'B'),
         ('C', 'D'),
         ('C', 'E'),
         ('D', 'A'),
         ('D', 'B'),
         ('D', 'C'),
         ('D', 'E'),
         ('E', 'A'),
         ('E', 'B'),
         ('E', 'C'),
         ('E', 'D')]
         

As you can see we get a list of tuples/pairs of all combinations of the 5 different objects. There are a total of 20 pairs from running our code. 

Now, what if we were wanting to be very particular about the result and did not want any pairs that were already present. For example, ('A', 'E') might be considered the same as ('E', 'A') and we did not want both of these to be in our final result as they are considered the same. Now we can resolve this issue by using math and data structures such that we are selective about how we handle our elements (or 'objects') using data structures and use math to verify what is considered the same and what is not considered the same. I will provide further examples and readings about this further below, but for now I will jump into a Python tool called Itertools design to optimize and handle these kind of challenges.

## Itertools

        import itertools

        pairs = list(itertools.combinations(group, 2))
        pairs
        
        [('A', 'B'),
         ('A', 'C'),
         ('A', 'D'),
         ('A', 'E'),
         ('B', 'C'),
         ('B', 'D'),
         ('B', 'E'),
         ('C', 'D'),
         ('C', 'E'),
         ('D', 'E')]
         
         
And as simple as 3 lines of code, we have our solution. In fact, using itertools actually performs faster than our previous example using two nested for-loops.

To use itertools.combinations(iterable, r) you need to pass into the first argument/parameter an iterable object (i.e. list, tuple, etc.) and the second argument 'r' is an integer representing the length of the subsequence (in our case it is 2 since we just wanted pairs). 

Note that there is another function in itertools called permutations that would have generated the same result as the two nested for-loops in our first attempt. Permutations is NOT concerned about order so ('A','B') is considered different from ('B','A'). So you have the option of choosing between the combinations function and the permutations function. Documentation for these and other functions in itertools can be found [here] (https://docs.python.org/3/library/itertools.html#itertools.combinations).

## Applying Itertools to German Election/Coalition Formation

Lets use Itertools for a more tangible exercise. The German elections goes through a number of phases. (As of writing this tutorial) Germany jsut elected its representatives to its parliment. There are currently six major parties and independents/others for a total of 7 "groups" to consider for this example. This recent election was considered a watershed or turning point for the CDU/CSU party as they maintain control for over a decade with Angela Merkel serving as Chancellor of Germany for over 15 years.  With the recent election the SPD party usurped the largest control of the seats in parliament over CDU/CSU. In the next few months Germany's Parliament will now go through proceedings to select its Chancellor from one of the leaders from the major parties. This requires all 7 groups to go through strategic negotiations to find alliances amongst one another and form coalitions that will garner it over 50% of the votes/seats in order to determine their Chancellor. 

So lets use this event as an exercise to figure out all the possible (though many are unreasonable) coalitions that could be formed among the 6/7 parties/groups controlling the seats in parliment. So lets see how many 2-party, 3-party, 4-party, and 5- party coalitions could be formed.

        coalition_dict = {}
        party = ['CDU_CSU', 'SPD', 'Green', 'FDP', 'Left', 'AfD', 'Others']
        for i in range(2,6):
        coalition_dict[i] = list(itertools.combinations(party, i))
    
        coalition_dict
        
We are not just interested in all the possible 2 to 5 party coalitions, but the possible ones that have over 50% of the seats. We do not care for coalitions that have less than 50%. So what we will do is iterate through all the entries in the dictionary and verify if they can provide over 50% of the seats/votes. If they can we will create another dictionary for these results. In actual practice we can edit our code above or just use the current dictionary we have. I will use the current dictionary we have for the sake of continuity of this exploration. 

First I will create a new dictionary to hold the percentage of seats each party controls in Germany's parliament.

        german_party_dict = {}
        party = ['CDU_CSU', 'SPD', 'Green', 'FDP', 'Left', 'AfD', 'Others']
        seat_pct = [24.1, 25.7, 14.8, 11.5, 4.9, 10.3, 8.7]

        for i,x in enumerate(party):
            german_party_dict[x] = seat_pct[i]

        german_party_dict
        
Now I will iterate through the recent coalition dictionary we built and tally up the total percentage.

        viable_coalition = {}
        for key, value in coalition_dict.items():
            for x in value:
                tmp_pct = 0.0
                for y in x:
                    tmp_pct += german_party_dict[y]
                if (tmp_pct > 50.0):
                    viable_coalition[key] = value

        viable_coalition
        
Now we have a dictionary of all the possible coalitions that can collect more than 50% of the votes in parliment. While these 91 coalitions are possible, they are unlikely to form due to factors outside of this tutorial (political, ideological, economics, etc.). 

## Analyzing the Composition of the Coalition
While we will not (yet) go into analyzing the potential formation of the 91 possible coalitions, we will explore the 'Power' of the party in each of the coalitions. Suppose we were interested in seeing how a certain party holds 'power' in the various combinations of coalitions. That is with all the possible coalitions that can be formed, when does the absence of one of the parties result in the the remaining parties to be unable to maintain over 50% of the seats/votes.

        power_dict = {}
        for x in party:
            power_dict[x] = []
        for key, value in viable_coalition.items():
            for x in value:
                for y in x:
                    total = 0.0
                    tmp = list(x)
                    tmp.remove(y)
                    for i in tmp:
                        total+=german_party_dict[i]
                    if (total < 50.0):
                        power_dict[y].append(x)
        power_dict
        
We now have a organized collection of instances when a certain party was critical to the formation of a coalition. Lets count the number of occurrences to computer the 'Power' of that party. We can simply just use the len() function to find out how large the list is to get a total size of the list of coalitions a party was critical to.

        total = 0
        party_power_count = {}
        for key, value in power_dict.items():
            party_power_count[key] = len(value)
            total += len(value)

        party_power_count

        {'CDU_CSU': 39,
         'SPD': 41,
         'Green': 35,
         'FDP': 33,
         'Left': 29,
         'AfD': 31,
         'Others': 29}

But, on second thought maybe it might be useful later to know how many times a certain party was critical to the formation of a coalition when the coalition was of a certain size. Since the coalition size range from 3 to 5 this won't be too tedious to do. I will just rewrite the previous code with some extra lines to tally the occurrences.

        total = 0
        party_power_count = {}
        for key, value in power_dict.items():
            tmp = [0,0,0]
            for x in value:
                if len(x) == 3:
                    tmp[0]+=1
                elif len(x) == 4:
                    tmp[1]+=1
                else:
                    tmp[2]+=1
            party_power_count[key] = [len(value),tmp]
            total += len(value)

        party_power_count

        {'CDU_CSU': [39, [15, 18, 6]],
         'SPD': [41, [15, 19, 7]],
         'Green': [35, [15, 16, 4]],
         'FDP': [33, [15, 15, 3]],
         'Left': [29, [15, 13, 1]],
         'AfD': [31, [15, 14, 2]],
         'Others': [29, [15, 13, 1]]}
         
         
We can see that most of the counts/occurrences of when a party was critical was when it was a 3-party coalition. We only begin to see some slight variation when we have 4 and 5-party coalitions.

