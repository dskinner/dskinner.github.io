+++
date = "2008-10-23T16:54:00-05:00"
title = "IBM Sliding Puzzle Contest"
+++

So someone passed onto me a pdf for an IBM sliding puzzle contest. Basically, it consists of a 3 row by 3 column puzzle with one empty space, you know the ones, and you have to write a piece of software that solves for the answer. The instructional pdf suggests that while its acceptable for your answer to be over 20 moves, it should optimally be about 20 or less and be relatively quick.

At first I had no clue about how to do something like this but found the idea very interesting. Four hours later I had a python script that solves for all possible solutions up to how ever many moves you choose it too. Turns out the shortest answer is 12 moves, according to my script. I haven't validated the 12 move answer, but i did validate a 14 move answer by hand with success (which was actually a twelve move answer with a repeat move making it 14), and i see little reason for the 12 move answer to be wrong.

Anyway, here it is in all its glory

```
puzzle = ['0', '4', '2', '5', '8', '3', '1', '7', '6']

answers = []

possible_moves = [
    [lambda p: swap(p, 1, 0), lambda p: swap(p, 3, 0)],
    [lambda p: swap(p, 0, 1), lambda p: swap(p, 2, 1), lambda p: swap(p, 4, 1)],
    [lambda p: swap(p, 1, 2), lambda p: swap(p, 5, 2)],
    [lambda p: swap(p, 0, 3), lambda p: swap(p, 4, 3), lambda p: swap(p, 6, 3)],
    [lambda p: swap(p, 1, 4), lambda p: swap(p, 3, 4), lambda p: swap(p, 5, 4), lambda p: swap(p, 7, 4)],
    [lambda p: swap(p, 2, 5), lambda p: swap(p, 4, 5), lambda p: swap(p, 8, 5)],
    [lambda p: swap(p, 3, 6), lambda p: swap(p, 7, 6)],
    [lambda p: swap(p, 4, 7), lambda p: swap(p, 6, 7), lambda p: swap(p, 8, 7)],
    [lambda p: swap(p, 5, 8), lambda p: swap(p, 7, 8)]
]

def serialize(p):
    return ''.join(p)

def swap(L, m, t):
    if L[t] is '0':
        L[t] = L[m]
        L[m] = '0'
        return serialize(L)
    
def no_dups(S):
    if len(S.split("-")) != len(set(S.split("-"))):
        return False
    else:
        return True

def search(tree, index=0):
    if index &lt; 12:
        for each in tree:
            for i, val in enumerate(list(each.split("-")[-1])):
                if val is '0':
                    generation = []
                    for move in possible_moves[i]:
                        result = each+"-"+move(list(each.split("-")[-1]))
                        if "123456780" in result:
                            answers.append(result)
                        elif no_dups(result):
                            generation.append(result)
                    search(generation, index+1)

search(["-"+serialize(puzzle)])
shortest_answer = sorted(answers)[0]
print "=========="
print "Shortest Answer: " + str(len(shortest_answer.split("-"))-2) + " Moves"
print "++++++++++"
print shortest_answer
```

Edit: I just thought i would add, 
yeah so the thing that was the turning point for solving this thing was how you look at solving the puzzle. Ive always looked at those puzzles as, ok what can i move into the empty space and rotate these pieces around and this and that but thats totally the wrong viewpoint. The way to visualize solving the answer is to look at the empty space as your focus and to move the empty space around, pushing the other numbers around into place. The puzzle suddenly becomes eaiser to solve by hand on your own and thats how the program solves for the answer too, by moving the empty space around, not trying to calculate how to get a particular number to its destination
