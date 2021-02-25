```python
import random
suits = ('Hearts', 'Diamonds', 'Spades', 'Clubs')
ranks = ('Two', 'Three', 'Four', 'Five', 'Six', 'Seven', 'Eight', 'Nine', 'Ten', 'Jack', 'Queen', 'King', 'Ace')
values = {'Two':2, 'Three':3, 'Four':4, 'Five':5, 'Six':6, 'Seven':7, 'Eight':8, 'Nine':9, 'Ten':10, 'Jack':10,
         'Queen':10, 'King':10, 'Ace':11}
```


```python
# Build card class

class Card():
    
    def __init__(self,rank,suit):
        self.rank = rank
        self.suit = suit
        self.value = values[rank]
        
    def __str__(self):
        return f'{self.rank} of {self.suit}'
```


```python
# Build a new deck with shuffle function

class Deck():
       
    def __init__(self):
        self.cards = []
        for i in suits:
            for k in ranks:
                self.cards.append(Card(k,i))
            
    def shuffle(self):
        random.shuffle(self.cards)
    
    def deal(self):
        return self.cards.pop()
    
    def noofcards(self):
        return len(self.cards)
```


```python
# Build a player class

class Player():
    
    def __init__(self,name,balance):
        self.name = name
        self.balance = balance
    
    def win(self,value):
        self.balance += value
    
    def lose(self,value):
        self.balance -= value
```


```python
# Build a hand class

class Hand():
    
    def __init__(self):
        self.cards = []
    
    def totalval(self):
        val = 0
        nooface = 0
        for i in self.cards:
            val += i.value
            if i.rank == 'Ace': nooface += 1
        while val > 21 and nooface > 0:
            val -= 10
            nooface -= 1
        return val
    
    def display(self):
        for i in self.cards:
            print(i)
```


```python
pname = input('What is your name? ')

while True:
    pbal = input('How much money in your balance? ')
    if pbal.isnumeric() == False or int(pbal) <= 0 or pbal.find('.') != -1:
        print('Please input a positive whole NUMBER!')
        continue
    else:
        pbal = int(pbal)
        print(f'Welcome to the game, {pname}! Your current balance is ${pbal}')
        break

player = Player(pname,pbal)

gameon = True
while gameon == True:
    
    newdeck = Deck()
    newdeck.shuffle()
    newdeck.shuffle()
    newdeck.shuffle()
    
    makebet = True
    while makebet:
        pbet = input('How much money do you want to bet for this game? ')
        if pbet.isnumeric() == False or int(pbet) <= 0 or pbet.find('.') != -1:
            print('Please input a positive whole NUMBER!')
        elif int(pbet) > player.balance:
            print(f'Not sufficient fund for this bet. You only have ${player.balance} left!')
        else:
            pbet = int(pbet)
            print(f'Your bet for this game is ${pbet}')
            makebet = False
    
    dehand = Hand()
    dehand.cards.append(newdeck.deal())
        
    phand = Hand()
    phand.cards.append(newdeck.deal())
                  
    dehand.cards.append(newdeck.deal())
    phand.cards.append(newdeck.deal())
    
    print(f'Dealer hand is {dehand.cards[0]} and ???.')
    print('Your hand is:')
    phand.display()
    print(f'Your total value is {phand.totalval()}')
          
    ans = 'x'
    handno = 2
    while ans not in ['H','h','F','f']:
        if handno == 5:
            ans = 'F'
        else:
            ans = input('Do you want to hit or fold? Type H or F: ')
        if ans == 'H' or ans == 'h':   
            phand.cards.append(newdeck.deal())
            print('Your hand is:')
            phand.display()
            print(f'Your total value is {phand.totalval()}')
            handno += 1
            if phand.totalval() > 21:
                print(f'You busted, {pname}!')
                player.lose(pbet)
                print(f'You lost ${pbet}! Your current balance is ${player.balance}')
                if player.balance == 0:
                    print('You have lost all your money. Please get out!')
                    gameon = False
                    break
                else:
                    ans2 = 'x'
                    while ans2 not in ['Y','y','N','n']:
                        ans2 = input('Do you want to play again? Y or N: ')
                        if ans2 == 'Y' or ans2 == 'y':
                            gameon = True
                            continue
                        if ans2 == 'N' or ans2 == 'n':
                            gameon = False
                            print('Have a good day!')
                            break
            else:
                ans = 'x'
        if ans == 'F' or ans == 'f':
            print('Your hand is:')
            phand.display()
            print(f'Your total value is {phand.totalval()}.')
            print('Dealer hand is:')
            dehand.display()
            print(f'Dealer total value is {dehand.totalval()}.')
            dehandno = 2
            while dehand.totalval() < 21 and dehandno < 5 and dehand.totalval() <= phand.totalval():
                print('Dealer hit!')
                dehand.cards.append(newdeck.deal())
                dehandno += 1
                print('Dealer hand is:')
                dehand.display()
                print(f'Dealer total value is {dehand.totalval()}.')
            if dehand.totalval() > 21 or dehand.totalval() <= phand.totalval():
                print('Dealer lost!')
                player.win(pbet)
                print(f'You won ${pbet}! Your current balance is ${player.balance}')
                ans2 = 'x'
                while ans2 not in ['Y','y','N','n']:
                    ans2 = input('Do you want to play again? Y or N: ')
                    if ans2 == 'Y' or ans2 == 'y':
                        gameon = True
                        continue
                    if ans2 == 'N' or ans2 == 'n':
                        gameon = False
                        print('Have a good day!')
                        break
            else:
                print(f'You lost, {pname}!')
                player.lose(pbet)
                print(f'You lost ${pbet}! Your current balance is ${player.balance}')
                if player.balance == 0:
                    print('You have lost all your money. Please get out!')
                    gameon = False
                    break
                else:
                    ans2 = 'x'
                    while ans2 not in ['Y','y','N','n']:
                        ans2 = input('Do you want to play again? Y or N: ')
                        if ans2 == 'Y' or ans2 == 'y':
                            gameon = True
                            continue
                        if ans2 == 'N' or ans2 == 'n':
                            gameon = False
                            print('Have a good day!')
                            break
```


```python

```
