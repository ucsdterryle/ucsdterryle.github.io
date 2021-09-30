# Using Random to Simulate Card Games (Poker and Blackjack)

In this tutorial, we will explore a number of different ways to try to simulate the basics of using a deck of cards to play casino games like Poker and Blackjack. A deck of cards is often used in probability courses to demonstrate basic probability concepts since a shuffled deck of cards is expected to have some random order where each of the 52 cards in the deck has an equal probability of being drawn from the deck. However, the problem with simulating this basic property of a shuffled deck of cards is the 'true' sense of random. Essential the concept of random is complicated when we rely on a machine to perform this function for us. I won't get into much detail about the 'randomness' property in computers but I will make comments along the way as we work with different data structures to represent our deck of cards

## Basics of a Deck of Cards
A (single) deck of cards is a set of 52 cards that have values represented by numerical values and face cards: 'A','2','3','4','5','6','7','8','9','10','J','Q','K'. Each card value has four distinct suits: 'C'(lubs), 'D'(iamonds), 'H'(earts), 'S'(pade).

## Creating a Deck of Cards as a LIST
We will start this tutorial by populating a list with elements/items that represents a unique card value and suit. While we can 'easily' (or more appropriately, directly) just type out every unique 52 card as strings, we will use the efficiency of loops to help us build the 52 cards for our deck.

        suit = ['C', 'D', 'H', 'S']
        value = ['A','2','3','4','5','6','7','8','9','10','J','Q','K']
        deck = []
        for x in suit:
          for y in value:
            tmp = [y,x]
            deck.append(tmp)
            
        deck
        
Our deck of cards is represented by a list that has elements that are a list of 2 strings the first representng a value and the second representing the suit. The key property to be aware of is that this list is ordered by the sequence the element/'card' was added to the list. Thus, like a fresh deck of cards that you buy from a store, the deck at the end of the code above is ordered and not random. 

Another data structures to use for our individual cards are tuples.

        suit = ['C', 'D', 'H', 'S']
        value = ['A','2','3','4','5','6','7','8','9','10','J','Q','K']
        deck = []
        for x in suit:
          for y in value:
            tmp = (y,x)
            deck.append(tmp)
            
        deck
        
## Creating a Deck of Cards as a Dictionary

## Creating a Deck of Cards as a Set

## Creating a Deck of Cards as a Tuple
While I did create a section for this data structure, this wouldn't be possible. Since the idea of using a deck of cards requires us to remove a card each time a card is drawn, a tuple would not be able to fulfill this action since a tuple is immutable and that we are unable to change the elements in the tuple. 


## Simulating a Poker Table


## Simulating a Blackjack Casino Table
Online gambling has become a prominent activity, bringing the action found in casinos to the comfort of your home or anywhere you have a computer. At one time, my roommate was engaged in playing online Blackjack and was concerned about a losing streak he encountered and was suspicious to the integrity of online gambling.  One thing we can try to do is simulate hundreds or thousands of sequential Blackjack games to see the likelihood of certain events occurring. 
