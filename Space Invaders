import math
import random

import pygame
from pygame import mixer

win = pygame.display.set_mode((800,600))
win.fill((255,255,255))

# Oyunu Baslatma
pygame.init()

# Ekran yaratma
screen = pygame.display.set_mode((800, 600))

# Arkaplan
background = pygame.image.load('arkaplan.png')

# Ses
mixer.music.load("arkaplansesi.wav")
mixer.music.play(-1)

# Yukardaki Ikon
pygame.display.set_caption("Uzay Gemisi Oyunu - Kerem Puskullu")
icon = pygame.image.load('launch.png')
pygame.display.set_icon(icon)

# Uzay gemisi
playerImg = pygame.image.load('uzaygemisi.png')

#Mermi Resmi
bulletImg = pygame.image.load('mermi.png')

textX = 10
testY = 10

# Kaybettiniz Yazisi
over_font = pygame.font.Font('freesansbold.ttf', 64)

# Skor Arttirma

score_value = 0
font = pygame.font.Font('freesansbold.ttf', 32)

# dusman
enemyImg = []
enemyX = []
enemyY = []
enemyX_change = []
enemyY_change = []
num_of_enemies = 7

# Karakter Konumlari
playerX = 370
playerY = 480
playerX_change = 0

# Mermi

# Ready - oldugunda mermi ekranda gozukmez
# Ates - Mermi hareket eder

bulletX = 0
bulletY = 480
bulletX_change = 0
bulletY_change = 10
bullet_state = "ready"

for i in range(num_of_enemies):
    enemyImg.append(pygame.image.load('dusman.png'))
    enemyX.append(random.randint(0, 736))
    enemyY.append(random.randint(50, 150))
    enemyX_change.append(14)
    enemyY_change.append(40)

class button():
    def __init__(self, color, x, y, width, height, text=''):
        self.color = color
        self.x = x
        self.y = y
        self.width = width
        self.height = height
        self.text = text

    def draw(self, win, outline=None):
        #Butonu ekrana cizmek icin kullandim
        if outline:
            pygame.draw.rect(win, outline, (self.x - 2, self.y - 2, self.width + 4, self.height + 4), 0)

        pygame.draw.rect(win, self.color, (self.x, self.y, self.width, self.height), 0)

        if self.text != '':
            font = pygame.font.SysFont('comicsans', 60)
            text = font.render(self.text, 1, (0, 0, 0))
            win.blit(text, (
            self.x + (self.width / 2 - text.get_width() / 2), self.y + (self.height / 2 - text.get_height() / 2)))

    def isOver(self, pos):
        # Pos is the mouse position or a tuple of (x,y) coordinates
        if pos[0] > self.x and pos[0] < self.x + self.width:
            if pos[1] > self.y and pos[1] < self.y + self.height:
                return True

        return False

def redrawwindow():
    butonum.draw(win,(0,0,0))

def show_score(x, y):
    score = font.render("Skor : " + str(score_value), True, (255, 255, 255))
    screen.blit(score, (x, y))


def game_over_text():
    over_text = over_font.render("KAYBETTİNİZ!", True, (255, 255, 255))
    screen.blit(over_text, (180, 250))

def player(x, y):
    screen.blit(playerImg, (x, y))


def enemy(x, y, i):
    screen.blit(enemyImg[i], (x, y))


def fire_bullet(x, y):
    global bullet_state
    bullet_state = "fire"
    screen.blit(bulletImg, (x + 16, y + 10))

def isCollision(enemyX, enemyY, bulletX, bulletY):
    distance = math.sqrt(math.pow(enemyX - bulletX, 2) + (math.pow(enemyY - bulletY, 2)))
    if distance < 27:
        return True
    else:
        return False

#Oyundan Cikma Butonu
butonum = button((255, 255, 0), 250, 60, 300, 100, 'Oyundan Çık')

# Ana Kod

running = True
while running:
    # RGB = Red, Green, Blue
    screen.fill((0, 0, 0))
    # Arka Plan Fotosu
    screen.blit(background, (0, 0))
    for event in pygame.event.get():
        pos = pygame.mouse.get_pos()
        if event.type == pygame.QUIT:
            running = False

        if event.type == pygame.MOUSEBUTTONDOWN:
            if butonum.isOver(pos):
                quit()

        if event.type == pygame.MOUSEMOTION:
            if butonum.isOver(pos):
                butonum.color = ((0,255,0))
            else:
                butonum.color = ((255,255,0))

        # Saga sola hareketler ve mermı fırlatma spacele
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_LEFT:
                playerX_change = -5
            if event.key == pygame.K_RIGHT:
                playerX_change = 5
            if event.key == pygame.K_SPACE:
                if bullet_state is "ready":
                    bulletSound = mixer.Sound("lazersesi.wav")
                    bulletSound.play()
                    # X koordınatını alma
                    bulletX = playerX
                    fire_bullet(bulletX, bulletY)

        if event.type == pygame.KEYUP:
            if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
                playerX_change = 0

    playerX = playerX_change + playerX
    if playerX <= 0:
        playerX = 0
    elif playerX >= 736:
        playerX = 736

    # Dusman Hareketleri
    for i in range(num_of_enemies):

        # Oyunu Kaybettin
        if enemyY[i] > 440:
            for j in range(num_of_enemies):
                enemyY[j] = 2000
            game_over_text()
            redrawwindow()
            break

        enemyX[i] += enemyX_change[i]
        if enemyX[i] <= 0:
            enemyX_change[i] = 6
            enemyY[i] += enemyY_change[i]
        elif enemyX[i] >= 736:
            enemyX_change[i] = -6
            enemyY[i] += enemyY_change[i]

        # Mermi dusmana degdiginde
        collision = isCollision(enemyX[i], enemyY[i], bulletX, bulletY)
        if collision:
            explosionSound = mixer.Sound("patlamasesi.wav")
            explosionSound.play()
            bulletY = 480
            bullet_state = "ready"
            score_value = score_value + 1
            enemyX[i] = random.randint(0, 736)
            enemyY[i] = random.randint(50, 150)

        enemy(enemyX[i], enemyY[i], i)

    # Mermi Hareketi
    if bulletY <= 0:
        bulletY = 480
        bullet_state = "ready"

    if bullet_state is "fire":
        fire_bullet(bulletX, bulletY)
        bulletY -= bulletY_change

    player(playerX, playerY)
    show_score(textX, testY)
    pygame.display.update()
