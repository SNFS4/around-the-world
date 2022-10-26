import pygame
pygame.init()  
pygame.display.set_caption("easy platformer")  # sets the window title
screen = pygame.display.set_mode((800, 800))  # creates game screen
screen.fill((0,0,0))
clock = pygame.time.Clock() #set up clock
gameover = False #variable to run our game loop

#CONSTANTS
LEFT=0
RIGHT=1
UP = 2
DOWN = 3

background = pygame.image.load("C:/Users/770857/Desktop/17208426.jpg")


#player variables
xpos = 500 #xpos of player
ypos = 200 #ypos of player
vx = 0 #x velocity of player
vy = 0 #y velocity of player
keys = [False, False, False, False] #this list holds whether each key has been pressed
isOnGround = False #this variable stops gravity from pulling you down more when on a platform
health = 100

#enemy variables
#expos = 200
#eypos = 630
#timer = 0
enemy1= [200, 630, 0]
enemy2 = [400,780,45]
enemy3 = [470,430,70]


def enemyMove(enemyInfo):
    enemyInfo[2]+=1
    if enemyInfo[2] <= 80:
        enemyInfo[0] += 1
    elif enemyInfo[2] <= 160:
        enemyInfo[0] -=1
    else:
        enemyInfo[2] = 0
        
def enemyCollide(enemyInfo, playerX, playerY):
    if playerX+20>enemyInfo[0]:
        if playerX < enemyInfo[0]+20:
            if playerY < enemyInfo[1]+20:
                if playerY+20 > enemyInfo[1]:
                    return True
    else:
        return False

jump = pygame.mixer.Sound('C:/Users/770857/Downloads/Y2Mate.is - Mike Ehrmantraut  finger say Waltuh sound effect-JT23uK39BAY-128k-1654568025031.mp3')
#music = pygame.mixer.music.load('C:/Users/770857/Downloads/Y2Mate.is - Better Call Saul Main Title Theme (Extended)-5AI44gnaLxY-160k-1654318173865.mp3')
#pygame.mixer.music.play(-1)



while not gameover and health > 0: #GAME LOOP############################################################
    clock.tick(60) #FPS
    
    #Input Section------------------------------------------------------------
    for event in pygame.event.get(): #quit game if x is pressed in top corner
        if event.type == pygame.QUIT:
            gameover = True
      
        if event.type == pygame.KEYDOWN: #keyboard input
            if event.key == pygame.K_LEFT:
                keys[LEFT]=True

            elif event.key == pygame.K_UP:
                keys[UP]=True
                
            elif event.key == pygame.K_RIGHT:
                keys[RIGHT]=True
                
        elif event.type == pygame.KEYUP:
            if event.key == pygame.K_LEFT:
                keys[LEFT]=False

            elif event.key == pygame.K_UP:
                keys[UP]=False
            
            elif event.key == pygame.K_RIGHT:
                keys[RIGHT]=False
          
    #physics section--------------------------------------------------------------------
    #LEFT MOVEMENT
    if keys[LEFT]==True:
        vx=-3
        direction = LEFT
        
    elif keys[RIGHT]==True:
        vx=3
        direction = RIGHT
    #turn off velocity
    else:
        vx = 0
        #JUMPING
    if keys[UP] == True and isOnGround == True: #only jump when on the ground
        vy = -8
        isOnGround = False
        direction = UP
        pygame.mixer.Sound.play(jump)
    
    #function call for movement
    enemyMove(enemy1)
    enemyMove(enemy2)
    enemyMove(enemy3)
    
    
    if enemyCollide(enemy1, xpos, ypos) == True:
        print("hit!")
        health -= 10
    if enemyCollide(enemy2, xpos, ypos) == True:
        print("hit!")
        health -= 10
    if enemyCollide(enemy3, xpos, ypos) == True:
        print("hit!")
        health -= 10
    
    #COLLISION
    if xpos>100 and xpos<200 and ypos+40 >750 and ypos+40 <770:
        ypos = 750-40
        isOnGround = True
        vy = 0
    elif xpos>200 and xpos<300 and ypos+40 >650 and ypos+40 <670:
        ypos = 650-40
        isOnGround = True
        vy = 0
    elif xpos>300 and xpos<400 and ypos+40 >550 and ypos+40 <570:
        ypos = 550-40
        isOnGround = True
        vy = 0
    elif xpos>400 and xpos<500 and ypos+40 >450 and ypos+40 <470:
        ypos = 450-40
        isOnGround = True
        vy = 0
    elif xpos>500 and xpos<600 and ypos+40 >350 and ypos+40 <370:
        ypos = 350-40
        isOnGround = True
        vy = 0
    elif xpos>600 and xpos<700 and ypos+40 >250 and ypos+40 <270:
        ypos = 250-40
        isOnGround = True
        vy = 0
    elif xpos>700 and xpos<800 and ypos+40 >150 and ypos+40 <170:
        ypos = 150-40
        isOnGround = True
        vy = 0
    else:
        isOnGround = False


    
    #stop falling if on bottom of game screen
    if ypos > 760:
        isOnGround = True
        vy = 0
        ypos = 760
    
    #gravity
    if isOnGround == False:
        vy+=.2 #notice this grows over time, aka ACCELERATION
    

    #update player position
    xpos+=vx 
    ypos+=vy
    
  
    # RENDER Section--------------------------------------------------------------------------------
            
    screen.fill((0,0,0)) #wipe screen so it doesn't smear
    
    screen.blit(background, (50,100,))
    #draw player
    pygame.draw.rect(screen, (100, 200, 100), (xpos, ypos, 20, 40))
    #draw enamy
    pygame.draw.rect(screen, (225, 225, 0), (enemy1[0], enemy1[1], 20, 20))
    
    pygame.draw.rect(screen, (0, 225, 225), (enemy2[0], enemy2[1], 20, 20))
    
    pygame.draw.rect(screen, (225, 0, 225), (enemy3[0], enemy3[1], 20, 20))
    
    #first platform
    pygame.draw.rect(screen, (200, 0, 100), (100, 750, 100, 20))
    
    #second platform
    pygame.draw.rect(screen, (100, 0, 200), (200, 650, 100, 20))
    
    pygame.draw.rect(screen, (0, 0, 200), (300, 550, 100, 20))
    
    pygame.draw.rect(screen, (0, 200, 200), (400, 450, 100, 20))
    
    pygame.draw.rect(screen, (200, 0, 200), (500, 350, 100, 20))
    
    pygame.draw.rect(screen, (200, 200, 200), (600, 250, 100, 20))
    
    pygame.draw.rect(screen, (0, 50, 200), (700, 150, 100, 20))
    
    pygame.display.flip()#this actually puts the pixel on the screen
    
#end game loop------------------------------------------------------------------------------
if health <=0:
    print("game over, you died")
pygame.quit()
