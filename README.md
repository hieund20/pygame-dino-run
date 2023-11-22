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
    death_sound = pygame.mixer.Sound("resource/sounds/die.wav") # Set the sound when the dino have a collision with the cactus or ptera

    dino_icon = pygame.image.load("resource/images/dino_.png")
    pygame.display.set_icon(dino_icon)

    pygame.display.set_caption("Dino run")

    game_over = pygame.image.load("resource/images/game_over.png")
    replay_button = pygame.image.load("resource/images/replay_button.png")
    logo = pygame.image.load("resource/images/logo.png")

    GREY = (240, 240, 240)
```










