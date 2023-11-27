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

Define the ground class:

```
class Ground():
    def __init__(self):
        self.ground_length = 1202
        self.image1 = pygame.image.load("resource/images/ground.png")
        self.image1_x = 0
        self.image1_y = 200

        self.image2 = pygame.image.load("resource/images/ground.png")
        self.image2_x = self.image1_x + self.ground_length
        self.image2_y = self.image1_y

        self.speed = 4

    def draw(self, screen):
        screen.blit(self.image1, (self.image1_x, self.image1_y))
        screen.blit(self.image2, (self.image2_x, self.image2_y))

    def update(self):
        self.image1_x-=self.speed
        self.image2_x-=self.speed

        if self.image1_x + self.ground_length < 0:
            self.image1_x = self.image2_x + self.ground_length
        elif self.image2_x + self.ground_length < 0:
            self.image2_x = self.image1_x + self.ground_length
```

Define the clound class

```
class Cloud():
    def __init__(self):
        self.image = pygame.image.load("resource/images/cloud.png")
        self.WIDTH, self.HEIGHT = 70, 40
        self.image = pygame.transform.scale(self.image, (self.WIDTH, self.HEIGHT))
        self.speed = 1
        self.x = 600
        self.y = 50

    def update(self):
        self.x-=self.speed

        if self.x < -self.WIDTH:
            self.x = 600 # render again
            self.y = random.randint(10, 100)
    
    def draw(self, screen):
        screen.blit(self.image, (self.x, self.y))
```


Define the dino class
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

        self.hitbox = pygame.Rect(self.x + 5, self.y, self.WIDTH-15, self.HEIGHT-5) # pygame.Rect(left, top, width, height): pygame object for storing rectangular coordinates

        self.runImg1 = pygame.image.load("resource/images/dino_1.png") # Set the image of the dino when dino run
        self.runImg2 = pygame.image.load("resource/images/dino_2.png") # Set the image of the dino when dino run
        # runImg1, runImg2 will make animation of the dino when the dino run
        
        self.runImg1 = pygame.transform.scale(self.runImg1, (self.WIDTH, self.HEIGHT)) # Resize to new resolution of runImg1 follow WIDTH and HEIGHT
        self.runImg2 = pygame.transform.scale(self.runImg2, (self.WIDTH, self.HEIGHT)) # Resize to new resolution of runImg2 follow WIDTH and HEIGHT

        # Images of the dino when user press key down
        self.duck1 = pygame.image.load("resource/images/dino_ducking1.png") # Set the image of the dino when dino crouch
        self.duck2 = pygame.image.load("resource/images/dino_ducking2.png") # Set the image of the dino when dino crouch

        self.duck1 = pygame.transform.scale(self.duck1, (self.WIDTH+15, self.HEIGHT)) # Resize to new resolution of duck1 follow WIDTH and HEIGHT
        self.duck2 = pygame.transform.scale(self.duck2, (self.WIDTH+15, self.HEIGHT)) # Resize to new resolution of duck2 follow WIDTH and HEIGHT

        self.is_ducking = False

        self.duckImgs = [self.duck1, self.duck2]
        self.runImgs = [self.runImg1, self.runImg2]

        self.jump_sound = pygame.mixer.Sound("resource/sounds/jump.wav") # Set the sound when the dino jump
        self.count = 0 # Initial player score 
        self.jumping = False

    def jump(self):
        self.y-=self.up # Start jumping
        self.jumping=True
        self.jump_sound.play() # Play the sound when dino jump

    def update(self):
        if self.y < 170: # Check if jumping
            self.up = self.up + self.g * self.t # [1] Newton's First Law of Motion: v = u + at
            self.y -= self.up
            self.t += 0.12 # incrementing time

        if self.y > 170:  # Check if jumping is complete and resetting all
            self.y = 170
            self.t = 0
            self.up = 7
            self.jumping = False

        if self.is_ducking: # Check if the dino is crouch
            self.hitbox = pygame.Rect(self.x + 5, self.y + 20, self.WIDTH +12, self.HEIGHT -20)
            self.image = self.duckImgs[int(self.count)%2] # Set the current image of the dukcing dino. It is the result of the randomness of the count variable.
            self.count+=0.2

        elif self.jumping: # Check if the player press space after the dino crouch, I will change the dino image to the default image
            self.hitbox = pygame.Rect(self.x + 5, self.y, self.WIDTH - 15, self.HEIGHT-5) 
            self.image=self.Img # Modify the image of the dino. Set the default image

        else: # Check if the player don't press space, the dino will run normally
            self.hitbox = pygame.Rect(self.x + 5, self.y, self.WIDTH-17, self.HEIGHT-5)
            self.image = self.runImgs[int(self.count)%2] # Set the current image ofthe  running dino. It is the result of the randomness of the count variable.
            self.count += 0.15

    def draw(self, screen):
        screen.blit(self.image, (self.x, self.y))
        pygame.draw.rect(screen, (0, 0, 0), self.hitbox, 2) # Draw a rectangle. It will contain the dino image
```

<br/>
[1] Newton's First Law of Motion

Newton’s first law of motion states that:

> A body remains in the state of rest or uniform motion in a straight line unless and until an external force acts on it.

<br/>
If I don't add the Newton's First Law of Motion to the dino class, It will become:

https://github.com/hieund20/pygame-dino-run/assets/71435458/9ce5cde1-49d5-41ef-aab9-6e779a70e9c6

The reason is I had commentted this code line: <code>self.up = self.up + self.g * self.t # [1] Newton's First Law of Motion: v = u + at</code>

<br/>

In this game, I have two obstacles, there are: cactus and ptera (Pteranodon)

Define the Cactus class:

```
class Cactus():
    def __init__(self):
        self.image0 = pygame.image.load("resource/images/cacti-small.png")
        self.image1 = pygame.image.load("resource/images/cacti-big.png")
        self.width0 = 45
        self.height = 45
        self.width1 = 65

        self.image0 = pygame.transform.scale(self.image0, (self.width0, self.height))
        self.image1 = pygame.transform.scale(self.image1, (self.width1, self.height))

        self.is_cactus = True
        self.is_ptera = False

        self.image, self.width = random.choice([[self.image0, self.width0], [self.image1, self.width1]])
        
        self.x = random.randint(720, 1000)
        self.y = 175
        self.speed = 4

        self.hitbox = pygame.Rect(self.x, self.y, self.width, self.height)

    def update(self):
        self.x -= self.speed
        self.hitbox = pygame.Rect(self.x, self.y, self.width, self.height)
        
    def draw(self, screen):
        screen.blit(self.image, (self.x, self.y))
```
<br/>
Define the Ptera class:

```
class Ptera():
    def __init__(self):
        self.width, self.height = 50, 40
        self.im1 = pygame.image.load("resource/images/ptera1.png")
        self.im2 = pygame.image.load("resource/images/ptera2.png")

        self.im1 = pygame.transform.scale(self.im1, (self.width, self.height))
        self.im2 = pygame.transform.scale(self.im2, (self.width, self.height))

        self.flaps = [self.im1, self.im2]

        self.altitudes = [175, 150, 110]
        self.x = random.randint(750, 1000)
        self.y = random.choice(self.altitudes)

        self.speed = 5
        self.count = 0
        self.is_ptera = True
        self.is_cactus = False

        self.hitbox = (self.x, self.y+10, self.width, self.height-12)

    def update(self):
        self.image = self.flaps[int(self.count)%2] #make animation
        self.count+=0.1

        self.x -= self.speed
        if self.x < 50:
            self.allowed = True

        self.hitbox = pygame.Rect(self.x, self.y + 10, self.width, self.height)

    def draw(self, screen):
        screen.blit(self.image, (self.x, self.y))
        pygame.draw.rect(screen, (250, 0, 0), self.hitbox, 2)
```

Update the game fucntion:

```
def game():
    screen = pygame.display.set_mode((700, 250))
    clock = pygame.time.Clock()

    font = pygame.font.Font("freesansbold.ttf", 20)

    check_point = pygame.mixer.Sound("resource/sounds/checkpoint.wav")
    death_sound = pygame.mixer.Sound("resource/sounds/die.wav")

    dino_icon = pygame.image.load("resource/images/dino_.png")
    pygame.display.set_icon(dino_icon)

    pygame.display.set_caption("Dino run")

    game_over = pygame.image.load("resource/images/game_over.png")
    replay_button = pygame.image.load("resource/images/replay_button.png")
    logo = pygame.image.load("resource/images/logo.png")

    GREY = (240, 240, 240)

    ground = Ground()
    dino = Dino()
    cloud = Cloud()

    obstacles = [Cactus()]
    obstacles_start = time.time()
    minimum_time = 1.5

    running = False
    play_game = True
    dead = False
    high_score_value = 0
    FPS = 85

    while play_game:
        if not dead:
            screen.fill(GREY)
            ground.draw(screen)
            screen.blit(dino.image, (dino.x, dino.y))
            screen.blit(logo, (200, 70))
        
        pygame.display.update()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                play_game = False

            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_SPACE:
                    running = True
                    ground = Ground()
                    dino = Dino()
                    obstacles = [Cactus()]
                    obstacles_start = time.time()
                    dead = False
                    score_value = 0

        while running:
            clock.tick(FPS)
            score = font.render("Score: " + str(int(score_value)), True, (200, 200, 200))
            score_value+=0.25
            high_score_value=max(high_score_value, score_value)
            high_score = font.render("High Score: "+ str(int(high_score_value)), True, (200, 200, 200))
            screen.fill(GREY)

            # Event handling
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    pygame.quit()
                elif event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_SPACE:
                        dino.jump()
                    elif event.key == pygame.K_DOWN:
                        dino.is_ducking=True
                elif event.type == pygame.KEYUP:
                    if event.key == pygame.K_DOWN:
                        dino.is_ducking=False

            ground.update()
            ground.draw(screen)

            cloud.update()
            cloud.draw(screen)       

            dino.update()
            dino.draw(screen) 

            for obstacle in obstacles:
                if obstacle.is_cactus:
                    obstacle.speed = ground.speed
                elif obstacle.is_ptera:
                    obstacle.speed = ground.speed+1  
                obstacle.update()
                obstacle.draw(screen)

            screen.blit(score, (550, 30))
            screen.blit(high_score, (350, 30))

            # Add new obstacle
            if time.time() - obstacles_start > minimum_time + random.randrange(0, 30)/10:
                obstacles_start = time.time()
                
                if score_value > 500.0:
                    ptera_probability = random.random() # Generate a random float from 0 to 1.0
                    if ptera_probability > 0.5: # 20% probability that ptera is spawned
                        obstacles.append(Ptera())
                        obstacles[-1].speed = ground.speed+1
                    else:
                        obstacles.append(Cactus()) # 80% probability of a cactus (duh)
                        obstacles[-1].speed = ground.speed # Synchronise the speed
                else:
                    obstacles.append(Cactus())
                    obstacles[-1].speed=ground.speed
                
            if int(score_value) > 0 and int(score_value)%100==0 and int(score_value)%3==0: #increase speed after crosses is multiple of 300
                ground.speed+=0.25
                    
                for obstacle in obstacles:
                    if obstacle.is_cactus:
                        obstacle.speed = ground.speed
                    elif obstacle.is_ptera:
                        obstacle.speed = ground.speed + 1
                
            if score_value>1 and score_value%100==0: # Checkpoint sound after score crosses a multiple of 100
                check_point.play()

            if dino.hitbox.colliderect(obstacles[0].hitbox): # Collision detection with closest cactus
                death_sound.play()
                dead = True
                screen.blit(game_over, (170, 70))
                screen.blit(replay_button, (340, 100))

            if obstacles[0].x < -30:
                obstacles.pop(0)
                if obstacles==[]:
                    obstacles.append(Cactus())
                    obstacles_start=time.time()
            pygame.display.update()

            if dead:
                del dino
                del ground 
                del obstacles
                running = False
```

Finally, I call the game function at the bottom of the file:

```
game()
```

Now, the game is complete. I can run the game ny click on this button of Visual Studio Code:

![Untitled](https://github.com/hieund20/pygame-dino-run/assets/71435458/23b2c666-3742-44b8-8ed0-b7288ebc75fe)

Play the game:

https://github.com/hieund20/pygame-dino-run/assets/71435458/bc31dcfc-2062-46c6-b6ce-b40cb41d1371

Enjoy the coding !!


















