import threading
import numpy as np
import cv2
import time
import pygame
from pygame.locals import *
from random import randint

cap = cv2.VideoCapture(0)
cap.set(cv2.CAP_PROP_FRAME_WIDTH, 160)
cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 120)

pygame.init()
screen = pygame.display.set_mode((160*10,120*10))
image_trou = pygame.image.load("image-océan.jpg").convert()
image_trou = pygame.transform.scale(image_trou,(160*10,120*10))

image = pygame.image.load("image_poisson2.png")
image = pygame.transform.scale(image,(100,72))

image_bullet = pygame.image.load("image_bullet.png")
image_bullet = pygame.transform.scale(image_bullet,(50,24))

def findeboucle():
    global encore
    encore = False

def findeboucle2():
    global encore2
    encore2 = False

def event():
    encore2 = True
    timer = threading.Timer((0.01), findeboucle)
    timer.start()
    for event in pygame.event.get():
        if event.type == KEYDOWN:
            if event.key == K_q: 
                shoot.add(image_bullet,x-7,y+3,1)

def souris(cap):
    encore = True
    timer = threading.Timer((0.01), findeboucle)
    timer.start()


    _, frame = cap.read()
    hsv_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)

    # Green color
    low_green = np.array([70, 100, 72])
    high_green = np.array([90, 220, 255])
    green = cv2.inRange(hsv_frame, low_green, high_green)

    cv2.imshow("Red", green)
    cv2.imshow("Frame", frame)
    def coordonée(green):
        global x
        global y
        a = 0
        for i in green:
            a += 1
            try:
                x = (list(i).index(255))
                y = a
                return
            except:
                pass
    coordonée(green)
    key = cv2.waitKey(1)

class tir:
    vie = 20
    def __init__(self,objet,coordonée,équipe):
        self.objet = objet
        self.coordonée = coordonée
        self.équipe = équipe
    
    def trajectoire(self,x,y):
        if self.équipe == 1:
            self.coordonée = (self.coordonée[0]+60,self.coordonée[1])
        else:
            self.coordonée = (self.coordonée[0]-60,self.coordonée[1])
        if self.coordonée[0] >= 1700 or self.coordonée[0] <= -20:
            shoot.destroy(self)
        if self.coordonée[0] > (160-x-10)*10 and self.coordonée[0] < (160-x)*10 and self.coordonée[1] < (y+7)*10 and self.coordonée[1] > y*10:
            self.vie -= 1
            print(self.vie)
        self.list_coor.append(self.coordonée)

class Node:
    def __init__(self,data,nexte):
        self.data = data
        self.next = nexte
        
    def add(self,object,x,y,équipe):
        while self.next != None:
            self = self.next
        self.next = Node(tir(object,((160-x)*10,y*10),équipe),None)
        
    def destroy(self,balle):
        while self.next.data != balle:
            self = self.next
        self.next = self.next.next

    def calcul(self,x,y):
        tir.list_coor = []
        while self.next != None:
            self = self.next
            self.data.trajectoire(x,y)

            screen.blit(self.data.objet,self.data.coordonée)

shoot = Node(None,None)
x = 0
y = 0
d = 1
while True:
    souris(cap)
    screen.blit(image_trou,(0,0))
    shoot.calcul(x,y)
    screen.blit(image,((160-x)*10,(y*10)))
    pygame.display.flip()
    event()
    if d%3 == 0:
        d = 0
        shoot.add(pygame.transform.rotate(image_bullet, (180)),-2,randint(1,120),2)
    d+= 1

    
