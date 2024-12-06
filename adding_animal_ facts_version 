import random
import pygame
pygame.init()

animals_facts = {
    "Tiger": "Only about 4,500 remain in the wild due to poaching and habitat loss.",
    "Blue Whale": "The largest animal on Earth, reduced by 90% due to whaling.",
    "Gorilla": "All gorilla species are endangered, with only about 1,000 mountain gorillas left.",
    "Orangutan": "All three species are critically endangered due to habitat destruction.",
    "Rhino": "Some species have fewer than 100 individuals left in the wild.",
    "Elephant": "African forest elephants are critically endangered due to poaching.",
    "Pangolin": "The most trafficked mammal in the world, critically endangered.",
    "Sea Otter": "Population reduced to 2,000 by 1911, now slowly recovering."}


SCREEN_WIDTH, SCREEN_HEIGHT = 800, 800
white = (255, 255, 255)
black = (0,0,0)
gray = (128, 128, 128)
green = (0, 255, 0)
blue = (0,0,255)

#timer
fps = 60
timer = pygame.time.Clock ()

#board settings
rows = 4
cols = 4
board_x = 0
board_y = 100
board_width = SCREEN_WIDTH
board_height = SCREEN_HEIGHT - 200

correct = [[0 for _ in range(cols)] for _ in range(rows)]

options_list = []
spaces =[]
used = []     
new_board = True 
first_guess = False
second_guess = False
first_guess_num = 0 #change this later to fit game 
second_guess_num = 0 #change later
score = 0
best_score = 0
matches = 0
game_over = False


card_spacing = 10
card_width = (SCREEN_WIDTH // cols) - card_spacing 
card_height = ((SCREEN_HEIGHT-200) // rows) - card_spacing
 
def wrap_text(text, font, max_width):
    words = text.split()
    lines = []
    current_line = words[0]
    for word in words[1:]:
        test_line = current_line + " " + word
        if font.size(test_line)[0] <= max_width:
            current_line = test_line
        else:
            lines.append(current_line)
            current_line = word
    lines.append(current_line)
    return lines

#screen
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Endangered Species Memory Match Game")
title_font = pygame.font.SysFont('Arial' , 40)
small_font = pygame.font.SysFont('Arial' , 20)

def generate_board():
  global spaces
  global used 
  animals = list(animals_facts.keys())
  facts = list(animals_facts.values())
  spaces = animals + facts
  random.shuffle(spaces)

def draw_backgrounds():
  top_menu = pygame.draw.rect(screen, black, [0,0, SCREEN_WIDTH, 100], 0, 0) #the last zero is for a border variable and rounded edges
  title_text = title_font.render("Endangered Species Memory Match Game", True, white)
  screen.blit(title_text, (20,26))
  board_space = pygame.draw.rect(screen, gray, [board_x, board_y, board_width, board_height], 0 )
  botton_menu = pygame.draw.rect(screen, black, [0, SCREEN_HEIGHT - 100, SCREEN_WIDTH, 100], 0 )
  restart_button = pygame.draw.rect(screen, gray, [20, SCREEN_HEIGHT - 90, 200, 80])
  restart_text = title_font.render ('Restart', True, white)
  screen.blit(restart_text, (50,725))
  score_text = small_font.render (f'Current Turns: {score}', True, white)
  screen.blit(score_text, (550,725))
  best_text = small_font.render (f'Previous Best Score: {best_score}', True, white)
  screen.blit(best_text, (550,750))
  return restart_button

def draw_board():
  global rows 
  global cols
  global correct
  board_list = []
  for i in range (cols):
    for j in range (rows):
      x = board_x + i * (card_width + card_spacing) + card_spacing //2
      y = board_y + j * (card_height + card_spacing) + card_spacing // 2
      piece = pygame.draw.rect (screen, white, [ x, y, card_width, card_height], 0, 0)
      board_list.append(piece)

      #this is newest change 
      if correct [j][i] == 1:
        pygame.draw.rect(screen, green, [x - 3, y - 3, card_width + 6, card_height + 6], 3)
        
      text = spaces [i * rows + j]
      if len(text)> 20:
        wrapped_text = wrap_text(text, small_font, card_width-10)
        for k, line in enumerate(wrapped_text):
          piece_text = small_font.render(line, True, black if correct [j][i] == 1 else gray)
          text_width = piece_text.get_width()
          text_height = piece_text.get_height()
          text_x = x + (card_width - text_width)//2
          text_y = y + (card_height - len(wrapped_text)*text_height)//2 + k * text_height
          screen.blit(piece_text, (text_x,text_y))
      else:
          piece_text = small_font.render(text, True, black if correct[j][i] == 1 else gray)
          text_width = piece_text.get_width()
          text_height = piece_text.get_height()
          text_x = x + (card_width - text_width) // 2
          text_y = y + (card_height - text_height)//2 
          screen.blit(piece_text, (text_x, text_y))   
            
    #for r in range(rows):
      #for c in range(cols):
        #a = board_x + c * (card_width + card_spacing) + card_spacing //2
        #b = board_y + r * (card_height + card_spacing) + card_spacing // 2
        #if correct[r][c] == 1:
          #matched_text = spaces[c * rows +r]
          #wrapped_text = (wrap_text, small_font, card_width - 10)
          #for k, line in enumerate(wrapped_text):
            #piece_text = small_font.render(f'{spaces[c * rows + r]}', True, black)
            #text_width = piece_text.get_width()
            #text_height = piece_text.get_height()
            #text_a = a + (card_width - text_width) // 2
            #text_b = b + (card_height - text_height) // 2
            #screen.blit(piece_text, (text_a, text_b))
            #pygame.draw.rect(screen, green, [a - 3, b - 3, card_width + 6, card_height + 6], 3)

  return board_list
              

def check_guesses(first,second):
  global spaces
  global correct
  global score
  global matches
  if (spaces[first] in animals_facts and spaces[second] == animals_facts[spaces[first]]) or \
       (spaces[second] in animals_facts and spaces[first] == animals_facts[spaces[second]]):
    col1, row1 = first // rows, first % rows
    col2, row2 = second // rows, second % rows
    if correct[row1][col1] == 0 and correct[row2][col2] == 0:
        correct [row1][col1] = 1
        correct [row2][col2] = 1
        score += 1
        matches += 1
        print (correct)
        print(f"Matched: {spaces[first]} - {spaces[second]}")
  else: 
    score += 1
      



running = True 
while running:
  timer.tick(fps) 
  screen.fill(white) 
  if new_board:
    generate_board()
    print(spaces)
    new_board = False

  restart = draw_backgrounds()
  board = draw_board()

  if first_guess and second_guess:
    check_guesses(first_guess_num, second_guess_num)
    pygame.time.delay(1000)
    first_guess = False
    second_guess = False

  
    
  for event in pygame.event.get():
    if event.type == pygame.QUIT:
      running = False
    if event.type == pygame.MOUSEBUTTONDOWN:
      for i in range(len(board)):
        button = board[i]
        if not game_over:
            if button.collidepoint(event.pos) and not first_guess:
              first_guess = True
              first_guess_num = i
              print(i)
            if button.collidepoint(event.pos) and not second_guess and first_guess and i != first_guess_num:
              second_guess = True
              second_guess_num = i
              print(i)    
        
      if restart.collidepoint(event.pos):
        options_list = []
        used = []
        spaces = []
        new_board = True
        score = 0
        matches = 0
        first_guess = False
        second_guess = False 
        correct = [[0 for _ in range(cols)] for _ in range(rows)]
        game_over = False
      
    if matches == rows * cols // 2:
        game_over = True
        winner_box = pygame.draw.rect(screen, gray, [10, SCREEN_HEIGHT - 300, SCREEN_WIDTH - 20, 80], 0, 5) 
        winner_text = title_font.render(f'You Finished in {score} moves!', True, white) #Change to something more meaningful 
        screen.blit(winner_text, (20, SCREEN_HEIGHT - 290))
        if best_score > score or best_score == 0:
          best_score = score     

    if first_guess: 
      col = first_guess_num // rows
      row = first_guess_num % rows
      x = board_x + col * (card_width + card_spacing) + card_spacing // 2
      y = board_y + row * (card_height + card_spacing) + card_spacing // 2
      text = f'{spaces[first_guess_num]}'
      wrapped_text = wrap_text(text, small_font, card_width - 10)
      for i, line in enumerate(wrapped_text):
        piece_text = small_font.render(line, True, blue)
        text_width = piece_text.get_width()
        text_height = piece_text.get_height()
        text_x = x + (card_width - text_width) // 2
        text_y = y + (card_height - len(wrapped_text) * text_height) // 2 + i * text_height
        screen.blit(piece_text, (text_x, text_y))

    if second_guess: 
      col = second_guess_num // rows
      row = second_guess_num % rows
      x = board_x + col * (card_width + card_spacing) + card_spacing // 2
      y = board_y + row * (card_height + card_spacing) + card_spacing // 2
      text = f'{spaces[second_guess_num]}'
      wrapped_text = wrap_text(text, small_font, card_width - 10)
      for i, line in enumerate(wrapped_text):
        piece_text = small_font.render(line, True, blue)
        text_width = piece_text.get_width()
        text_height = piece_text.get_height()
        text_x = x + (card_width - text_width) // 2
        text_y = y + (card_height - len(wrapped_text) * text_height) // 2 + i * text_height
        screen.blit(piece_text, (text_x, text_y))
    

  
  pygame.display.flip()
pygame.quit()
 
