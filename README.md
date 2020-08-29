# War
import random

class Card:
    direction=['C', 'D', 'H', 'S']
    magnitude=['2', '3', '4', '5', '6', '7', '8', '9', 'T', 'J', 'Q', 'K', 'A']
    def __init__(self, value, suit):
        self.value=Card.magnitude.index(value.upper())
        self.suit=Card.direction.index(suit.upper())
    def __str__(self):
        return Card.magnitude[self.value]+' of '+Card.direction[self.suit]
    def __lt__(self, other):
        if self.value<other.value:
            return True
        elif self.value>other.value:
            return False

class Deck:
        deck=[]
        for i in Card.direction:
            for j in Card.magnitude:
                deck.append(j+i)
        random.shuffle(deck)
        def GiveDeck():
            return Deck.deck

class Player:
    playerid=1
    def __init__(self):
        self.name=input('player '+str(Player.playerid)+' name:')
        self.id=Player.playerid
        self.hand=[]
        self.pile=[]
        Player.playerid+=1
    def ChangeHand(self, newhand,leftovers=[]):
        self.hand=leftovers+newhand
        self.pile=[]
    def AddToPile(self, cards):
        self.pile.extend(cards)
        
class Game:
    def __init__(self):
        self.player1=Player()
        self.player2=Player()
    def DealHands(self):
        hand1=[]
        hand2=[]
        x=Deck.GiveDeck()
        counter=0
        while len(x)>0:
            if counter%2==0:
                hand1.append(x.pop(0))
            else:
                hand2.append(x.pop(0))
            counter+=1
        self.player1.ChangeHand(hand1)
        self.player2.ChangeHand(hand2)
    def Draw(self, atrisklist=[]):
        name1=self.player1.hand[0]
        name2=self.player2.hand[0]
        listofcards=atrisklist+[name1, name2]
        card1=Card(name1[0],name1[1])
        card2=Card(name2[0],name2[1])
        print(str(name1)+' vs '+str(name2))
        del self.player1.hand[0]
        del self.player2.hand[0]
        if card1<card2:
            self.player2.AddToPile(listofcards)
        elif card1>card2:
            self.player1.AddToPile(listofcards)
        else:
            print('oh fuck its a tie')
            self.Tie(listofcards)
    def Tie(self,Ties):
        print('ties enter as:', Ties)
        atrisk=[]
        if min(len(self.player1.hand),len(self.player2.hand))<4:
            x.PileToHand()
        for g in Ties:
            atrisk.append(g)
        for j in range(3):
            atrisk.append(self.player1.hand[0])
            atrisk.append(self.player2.hand[0])
            del self.player1.hand[0]
            del self.player2.hand[0]
        print('atrisk: '+str(atrisk))
        self.Draw(atrisk)
    def PileToHand(self):
        random.shuffle(self.player1.pile)
        random.shuffle(self.player2.pile)
        self.player1.ChangeHand(self.player1.pile, self.player1.hand)
        self.player2.ChangeHand(self.player2.pile, self.player2.hand)
    def PlayRound(self):
        print('cardsum:'+str(len(self.player1.hand)+len(self.player2.hand)))
        while min(len(self.player1.hand),len(self.player2.hand))>0:
            self.Draw()
        self.PileToHand()
        print('len 1:'+str(len(self.player1.hand)))
        print('len 2:'+str(len(self.player2.hand)))
    def Judge(self):
        if len(self.player1.hand)+len(self.player1.pile)>len(self.player2.hand)+len(self.player2.pile):
            print(str(self.player1.name)+' wins!!!!')
        else:
            print(str(self.player2.name)+' wins!!!!')
    def Execute(self):
        self.DealHands()
        while min(len(self.player1.hand+self.player1.pile),len(self.player2.hand+self.player2.pile))>0:
            try:
                self.PlayRound()
                print('----------------------------------')
            except IndexError:
                break
        self.Judge()
            
                
x=Game()
x.Execute()
