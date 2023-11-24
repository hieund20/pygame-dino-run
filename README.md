# pygame-dino-run

This is project that I create to learn pygame by making a simple game. I made this for my big assignment in Open Source Technology class.

Source: https://www.udemy.com/course/make-10-advanced-pro-games-in-python/

<br/>
<b>What is the pygame?</b> 

Pygame is a set of Python modules designed for writing video games.
Pygame adds functionality on top of the excellent SDL library.
This allows you to create fully featured games and multimedia programs in the python language.

See more at https://www.pygame.org/wiki/about

<br/>
<b>Coordinates in making game</b>

Different from normal coordinates in Two Dimensional Coordinate Geometry. Coordinates in making game is performed in reverse

![coordinate_image](https://github.com/hieund20/pygame-dino-run/assets/71435458/c1944135-c188-4143-bea0-738f8d0c5d4c?raw=true)

<br/>
<b>The IDE I used</b>

I used Visual Studio Code to make this project.

<br/>
<b>Project initialization</b>

Create venv

```
python -m venv venv
```

Python venv activation

```
# In cmd.exe
venv\Scripts\activate.bat
```

Install pygame

```
pip install pygame
```

<br/>
<b>File structure</b>

<br/>

![image](https://github.com/hieund20/pygame-dino-run/assets/71435458/bbf7db22-ad0a-4076-839a-04ce52611ce3)

images folder contain images of game 

sounds folder contain sounds of game 

venv folder is generated from command <code>python -m venv venv</code>

dino.py is the main file of project, I will write code then run project from this file.

<br/>
<b>Let's get started</b>

Import the neccessary libaries

```
import random
import pygame
from pygame.locals import *
import time
```

Initialize all imported pygame modules. Add it to your code at the top level:

```
pygame.init()
```

Define the game function

```
def game():
    screen = pygame.display.set_mode((700, 250)) # Set the width and height of the screen.
    clock = pygame.time.Clock()

    font = pygame.font.Font("freesansbold.ttf", 20) # Set the font of the game.

    check_point = pygame.mixer.Sound("resource/sounds/checkpoint.wav") # Set the sound when the dino jump.
    death_sound = pygame.mixer.Sound("resource/sounds/die.wav") # Set the sound when the dino have a collision with the cactus or ptera.

    dino_icon = pygame.image.load("resource/images/dino_.png") 
    pygame.display.set_icon(dino_icon) # Change the system image for the display window.

    pygame.display.set_caption("Dino run") # Set the current window caption.

    game_over = pygame.image.load("resource/images/game_over.png") # Set the title when player lose.
    replay_button = pygame.image.load("resource/images/replay_button.png") # Set the button that allow replay the game when player lose.
    logo = pygame.image.load("resource/images/logo.png") # Set the logo that will show when player lose.

    GREY = (240, 240, 240) # Set the background color

    # To be continued
```

Define the dino object
```
class Dino():
    def __init__(self):
        self.Img = pygame.image.load("resource/images/dino_.png") # Set image of the dino
        self.WIDTH, self.HEIGHT = 44, 48 # Set width and the heigth of dino
        self.Img = pygame.transform.scale(self.Img, (self.WIDTH, self.HEIGHT)) # Set image of the dino
        self.image = self.Img 
        self.x = 20 # Initial x position of the dino 
        self.y = 170 # Initial y position of the dino 
        self.g = -0.25 # Initial gravity of the dino
        self.up = 7 # Initial upward velocity of the dino
        self.t = 0 # time

        self.hitbox = pygame.Rect(self.x + 5, self.y, self.WIDTH-15, self.HEIGHT-5)

        self.runImg1 = pygame.image.load("resource/images/dino_1.png")
        self.runImg2 = pygame.image.load("resource/images/dino_2.png")
        self.runImg1 = pygame.transform.scale(self.runImg1, (self.WIDTH, self.HEIGHT))
        self.runImg2 = pygame.transform.scale(self.runImg2, (self.WIDTH, self.HEIGHT))

        # When user press key down
        self.duck1 = pygame.image.load("resource/images/dino_ducking1.png")
        self.duck2 = pygame.image.load("resource/images/dino_ducking2.png")
        self.duck1 = pygame.transform.scale(self.duck1, (self.WIDTH+15, self.HEIGHT))
        self.duck2 = pygame.transform.scale(self.duck2, (self.WIDTH+15, self.HEIGHT))

        self.is_ducking = False

        self.duckImgs = [self.duck1, self.duck2]
        self.runImgs = [self.runImg1, self.runImg2]

        self.jump_sound = pygame.mixer.Sound("resource/sounds/jump.wav")
        self.count = 0
        self.jumping = False

    def jump(self):
        self.y-=self.up # Start jumping
        self.jumping=True
        self.jump_sound.play()

    def update(self):
        if self.y < 170: # check if jumping
            self.up = self.up + self.g * self.t # Dinh ly 1 Newton: v = u + at
            self.y -= self.up
            self.t += 0.12 # incrementing time

        if self.y > 170:  # check if jumping is complete and resetting all
            self.y = 170
            self.t = 0
            self.up = 7
            self.jumping = False

        if self.is_ducking:
            self.hitbox = pygame.Rect(self.x + 5, self.y + 20, self.WIDTH +12, self.HEIGHT -20)
            self.image = self.duckImgs[int(self.count)%2]
            self.count+=0.2
        elif self.jumping:
            self.hitbox = pygame.Rect(self.x + 5, self.y, self.WIDTH - 15, self.HEIGHT-5)
            self.image=self.Img
        else:
            self.hitbox = pygame.Rect(self.x + 5, self.y, self.WIDTH-17, self.HEIGHT-5)
            self.image = self.runImgs[int(self.count)%2]
            self.count += 0.15

    def draw(self, screen):
        screen.blit(self.image, (self.x, self.y))
        pygame.draw.rect(screen, (0, 0, 0), self.hitbox, 2)
```














