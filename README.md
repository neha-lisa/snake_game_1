# snake_game_1
A snake game in python 

import pygame
import random
import os

# pygame.mixer.init()
# pygame.mixer.music.load('')
# pygame.mixer.music.play()


# define colors:
white=(255,255,255)
red=(255,0,0)
black=(0,0,0)
green=(0,255,0)
blue=(0,0,255)
pygame.init()

screen_width = 900
screen_height = 600
gameWindow = pygame.display.set_mode((screen_width,screen_height))
pygame.display.set_caption("Snake by Neha")
pygame.display.update()
# Clock
clock = pygame.time.Clock()
font = pygame.font.SysFont(None,55)

def text_screen(text, color, x ,y):
    screen_text = font.render(text,True,color)
    gameWindow.blit(screen_text, (x,y))

def plot_snake(gameWindow,color,snk_list ,snake_size):
    for x,y in snk_list:
        pygame.draw.rect(gameWindow, black, [x, y, snake_size, snake_size])
# welcome
def welcome():
    exit_game = False
    while not exit_game:
        gameWindow.fill(black)
        text_screen("Welcome to snakes",red,260,250)
        text_screen("Press space bar To Play",blue,235,290)
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                exit_game = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE:
                    gameloop()

        pygame.display.update()
        clock.tick(30)


#game Loop

def gameloop():
    # Game specific variables
    exit_game = False
    game_over = False

    snake_x = 45
    snake_y = 55
    snake_size = 15
    init_velocity = 5
    velocity_x = 0
    velocity_y = 0
    fps = 30
    score = 0
    food_x = random.randint(20, screen_width / 2)
    food_y = random.randint(20, screen_height / 2)

    snk_list = []
    snk_length = 1
    # Check if hiscore fiel exists
    if     (not os.path.exists("hiscore.txt")):
        with open("hiscore.txt","w") as f:
            f.write("0")
    with open("hiscore.txt", "r") as f:
        hiscore = f.read()

    while not exit_game:

        if game_over:
            gameWindow.fill(white)
            text_screen("Game Over ! press Enter to continue",red,100,250)
            with open("hiscore.txt", "w") as f:
                f.write(str(hiscore))

            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    exit_game = True
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_RETURN:
                        gameloop()
        else:
            for event in pygame.event.get():


                if event.type == pygame.QUIT:
                    exit_game = True


                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_RIGHT:
                        velocity_x = init_velocity
                        velocity_y = 0

                    if event.key == pygame.K_LEFT:
                        velocity_x = - init_velocity
                        velocity_y = 0

                    if event.key == pygame.K_UP:
                        velocity_y = - init_velocity
                        velocity_x = 0

                    if event.key == pygame.K_DOWN:
                        velocity_y = init_velocity
                        velocity_x = 0



            snake_x = snake_x + velocity_x
            snake_y = snake_y + velocity_y

            if abs(snake_x - food_x)<6 and abs(snake_y - food_y)<6:
                score += 10
                print("Score : ",score)
                food_x = random.randint(20, screen_width / 2)
                food_y = random.randint(20, screen_height / 2)
                snk_length += 5
                if score > int(hiscore):
                    hiscore = score


            gameWindow.fill(green)
            text_screen("Score: " + str(score)+"  Hiscore "+str(hiscore), blue, 3, 5)
            pygame.draw.rect(gameWindow, red, [food_x, food_y, snake_size, snake_size])

            head = []
            head.append(snake_x)
            head.append(snake_y)
            snk_list.append(head)


            if len(snk_list )> snk_length:
                del snk_list[0]

            if head in snk_list[:-1]:
                game_over = True

            if snake_x<0 or snake_x>screen_width or snake_y<0 or snake_y>screen_height:
                game_over = True
                # print("Game Over")

            # pygame.draw.rect(gameWindow, black, [snake_x,snake_y,snake_size,snake_size])
            plot_snake(gameWindow,black,snk_list ,snake_size)
        pygame.display.update()
        clock.tick(fps)



    pygame.quit()
    quit()


welcome()
