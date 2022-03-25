from pickle import FALSE, TRUE
from telnetlib import STATUS
from turtle import bgcolor
import pygame 
import numpy as np 
import time, os 


size = WIDTH, HEIGHT = 700, 700 # damos tamaño a nuestra pantalla 
nX, nY = 70, 70
xSize = WIDTH/nX
ySize = HEIGHT/nY

pygame.init() #iniciamos pygame 

screen = pygame.display.set_mode(WIDTH,HEIGHT) #damos tamaño a nuestra ventana  
bg_color= (0,0,0) # color dl fonfo 
live_color= (134,51,255) # color de las celulas vivas
dead_color= (14,47,98) # colores de las celulas muertas 

# celulas vivas = 1; celulas muertas = 0

# iniciamos el estado de las celulas 

status = np.zeros((nX,nY)) 

pausa=FALSE

running=TRUE

# automatas 
status[22,22] = 1
status[23,23] = 1
status[24,24] = 1
status[25,25] = 1
status[26,26] = 1
status[27,27] = 1

status[7,4] = 1
status[7,3] = 1
status[7,2] = 1
status[7,1] = 1
status[7,0] = 1

status[1,25] = 1
status[7,40] = 1
status[40,2] = 1

while running:
# evitamos que nuestra ventana se cierre en un ciclo infinito 
   newStatus = np.copy(status) # copiamos el status anterior 

   for event in pygame.event.get():
        if event.type == pygame.QUIT:
           running = False

        if event.type == pygame.KEYDOWN:
            pause = not pause 

        mouseClick = pygame.mouse.get_pressed()
        if sum(mouseClick) > 0:
            posX, posY = pygame.mouse.get_pos()
            x, y = int(np.floor(posX/xSize)). int(np.floor(posY/xSize))
            newStatus [x,y] = np.abs(newStatus[x,y]-1)
            newStatus [x,y] = not mouseClick[2]

screen.fill(bg_color) # limpiamos el fondo
time.sleep(0.1) # le damos una pausa de descanso al sistema 

# recorremos la matriz 

    for x in range(0,nX):
        for y in range(0,nY):


         if not pausa:

            #numeros de vecinos 
             nVeci = status[(x-1)%nX;(y-1)%nY] + status[(x)%nX,(y-1)%nY] + \
                    status[(x+1)%nX;(y-1)%nY] + status[(x-1)%nX,(y)%nY] + \
                    status[(x+1)%nX;(y)%nY] + status[(x-1)%nX,(y+1)%nY] + \
                     status[(x)%nX;(y+1)%nY] + status[(x+1)%nX,(y+1)%nY]

             # definimos las reglas

            # reglas 1: una celula muerta con tres vecinas vive 
             if status [x,y] == 0 and nVeci == 3:
                 newStatus[x,y] = 1

             #regala 2: una celula viva con mas tres vecinas o menos de 2 muere
             elif status[x,y] == 1  and (nVeci < 2 or nVeci > 3):
                newStatus[x,y] = 0


         poly = [(x*xSize,y*ySize),
                ((x+1)*xSize,y*ySize),
                ((x+1)*xSize,(y+1)*ySize)
                (x*xSize,(y+1)*ySize)]
        #dibujamos nuestro poligonos}

        if newStatus[x,y] == 1:
            pygame.draw.polygon(screen,live_color,poly,0)


       

status = np.copy(newStatus)
time.sleep(0.1)
pygame.display.flip()
pass



pygame.quit()

                        
