# Python-games
# implementation of card game - Memory

import simplegui
import random

# Define global variable
turn = 0
index1 = 0
index2 = 0
list1 = range(1, 9)
list2 = range(1, 9)
list1.extend(list2)
image = simplegui.load_image('https://c139491.ssl.cf0.rackcdn.com/photos/1429812/large.jpg')

# helper function to initialize globals
def new_game():
    global list1, dict, state, exposed, turn
    state = 0
    turn = 0
    random.shuffle(list1)
    dict = {20:list1[0], 70:list1[1], 120:list1[2], 170:list1[3], 220:list1[4], 270:list1[5], 320:list1[6], 370:list1[7], 420:list1[8], 470:list1[9], 520:list1[10], 570:list1[11], 620:list1[12], 670:list1[13], 720:list1[14], 770:list1[15]}
    exposed=[False,False,False,False,False,False,False,False,False,False,False,False,False,False,False,False]  
     
# define event handlers
def mouseclick(pos):
    global state, turn, exposed, index1, index2
# Define index1 for index of the first card exposed & index2 for the second card
# "index" being the place/order of the card by default
    index=pos[0]//50

# The game start, none cards exposed. 
# Then, First click, first exposed-> get a value for index1
# Second click, Kept First card index in memory, get a value for the second card
# Finally, if the two cards exposed are different, return a False for both... 
# And "index"  value is the new card clicked (=index1)
    if exposed[index] == False:
        exposed[index] = True
        if state == 0:
            state = 1
            index1 = index
        elif state == 1:
            state = 2
            index2 = index
        elif state == 2:
            if list1[index1] != list1[index2]:
                exposed[index1]= False
                exposed[index2]= False
            index1 = index
            state = 1 
            
        turn += 1 
                        
# cards are logically 50x100 pixels in size    
def draw(canvas):
    global turn, exposed
    label.set_text("Turn = "+str(turn))
    index = 0
# Draw 16 polygons to be nicer on the screen when cards are unexposed
    for card_pos in range(0, 801, 50):
        canvas.draw_polygon([(card_pos, 0), (card_pos+100, 0),
                             (card_pos+100, 100), (card_pos, 100)], 1, "white", "Black")

# Draw the 16 numbers (pairs) in a shuffle order
    for key, value in dict.items():
        canvas.draw_text(str(value), (key, 55), 30, "White")
        
# Draw 16 card's Pokemon("catch them Allllll") except for those who are exposed &/or discovered (exposed=True)
    for card in range(25, 801, 50):
        if exposed[index] == False:
            canvas.draw_image(image, (320, 452), (640, 905), (card, 50), (50, 100))
        index += 1

# create frame and add a button and labels
frame = simplegui.create_frame("Memory", 801, 101)
frame.add_button("Reset", new_game)
label = frame.add_label("Turns = 0")

# register event handlers
frame.set_mouseclick_handler(mouseclick)
frame.set_draw_handler(draw)

# get things rolling
new_game()
frame.start()
