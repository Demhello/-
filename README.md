from pygame import *
from random import *
FPS = 60
game = True
game2 = False
clock = time.Clock()
b = 1
h = 100
o = 0
n = 0
yr = 1
cocoin = 0
patron = 15
ap1 = 0
ap2 = 0
shop = False
pause1 = True


def endgame():
    global b,n,h,patron,inoplan,game,font1,cocoin,bul,shop,game2,o
    game2=True
    while game2 == True:
        scr.blit(back,(0,0))
        txt3 = "Вы проиграли"
        text3 = font2.render(txt3, 1, (255,0,0))
        colio = "Количество очков " + str(o)
        coliot = font3.render(colio, 1, (255,0,0))
        scr.blit(coliot, (50,150))
        scr.blit(text3, (400,100))

        for e in event.get():
            if e.type == QUIT:
                game2 = False
        display.update()

def shop1():
    global b,n,h,patron,inoplan,game,font1,cocoin,bul,shop,font2,font3,FPS,ap1,ap2
    shop = True
    while shop == True:
        keys_pressed = key.get_pressed()
        if keys_pressed[K_w]:
            game = True
            obrat()
        scr.blit(back,(0,0))
        button1 = knopka((0,255,0),300,80,235,20)
        button1.draw()
        button2 = knopka((0,255,0),300,110,320,20)
        button2.draw()
        button3 = knopka((0,255,0),300,140,330,20)
        button3.draw()
        txtc = "Количество монет "+str(cocoin)
        textc = font1.render(txtc,1,(255,0,0))
        txtsh ="Магазин"
        textsh = font2.render(txtsh,1,(255,0,0))
        pkup = "Купить 10 патронов(5 монет)"
        pkuptext = font1.render(pkup,1,(255,0,0))
        pkup1 = "купить 1 аптечку по 10 жизней(5 монет)"
        pkuptext1 = font1.render(pkup1,1,(255,0,0))
        pkup2 = "купить 1 аптечку по 50 жизней(15 монет)"
        pkuptext2 = font1.render(pkup2,1,(255,0,0))
        scr.blit(pkuptext, (300,80))
        scr.blit(pkuptext2, (300,140))
        scr.blit(pkuptext1, (300,110))
        scr.blit(textc, (10,60))
        scr.blit(textsh, (450,20))
        for e in event.get():
            if e.type == QUIT:
                shop = False
            elif e.type == MOUSEBUTTONDOWN:
                x, y = e.pos
                if button1.rect.collidepoint(x, y):
                    if cocoin >= 5:
                        patron += 10
                        cocoin -= 5
                if button2.rect.collidepoint(x, y):
                    if cocoin >= 5:
                        ap1 += 1
                        cocoin -= 5
                if button3.rect.collidepoint(x, y):
                    if cocoin >= 15:
                        ap2 += 1
                        cocoin -= 15
        clock.tick(FPS)
        display.update()
    
def pause():
    global b,n,h,patron,inoplan,game,font1,cocoin,bul,shop,endgame,pause1,yr
    pause1 = True
    while pause1 == True:
        scr.blit(back,(0,0))
        pausetxt = "Пауза"
        pausetext = font2.render(pausetxt, 1, (255,0,0))
        ypravtxt = "Управление: A - двигаться в лево, D - двигаться в право, S - вызвать магазин, W - вернуться из магазина"
        ypravtext = font1.render(ypravtxt, 1, (255,0,0))
        button4 = knopka((0,255,0),450,400,125,25)
        button4.draw()
        vertxt = "Вернуться"
        vertext = font3.render(vertxt, 1, (255,0,0))
        scr.blit(vertext,(450,400))
        scr.blit(ypravtext, (50,150))
        scr.blit(pausetext, (450,100))
        for e in event.get():
            if e.type == QUIT:
                pause1 = False
                game=False
            elif e.type == MOUSEBUTTONDOWN:
                    x, y = e.pos
                    if button4.rect.collidepoint(x, y):
                        pause1 = False
                        game = True   
    
            
        display.update()

#mixer.init()
#mixer.music.load("space.ogg")
#mixer.music.play()

scr = display.set_mode((1000,600))
back = transform.scale(image.load("galaxy.jpg"),(1000,600))

class obekt(sprite.Sprite):
    def __init__(self, picimage, sx,sy,sspeed):
        super().__init__()
        self.image = transform.scale(image.load(picimage),(90,80))
        self.speed = sspeed
        self.rect = self.image.get_rect()
        self.rect.x = sx
        self.rect.y = sy
    def reset(self):
        scr.blit(self.image,(self.rect.x,self.rect.y))

class knopka():
    def __init__(self,color,sx,sy,width,height):
        super().__init__()
        self.color = color
        self.width = width
        self.height = height

        self.image = Surface((self.width,self.height))
        self.image.fill((self.color))
        self.rect = self.image.get_rect()
        self.rect.x = sx
        self.rect.y = sy
    def draw(self):   
        scr.blit(self.image,(self.rect.x,self.rect.y))


class bull(obekt):
    def fire(self):
        global b
        if self.rect.y > -50:
            self.rect.y -= self.speed
        else:
            self.rect.y = -1000
            b = 1

bul = bull("bullet.png", -100,-100, 10)

class igrok(obekt):
    def update(self):
        global b,patron,n,game,shop
        keys_pressed = key.get_pressed()
        if keys_pressed[K_d] and self.rect.x < 900:
            self.rect.x += 5
        if keys_pressed[K_a] and self.rect.x > 5:
            self.rect.x -= 5
        if keys_pressed[K_SPACE] and b == 1 and patron > 0:
            bul.rect.x = rocket.rect.x
            bul.rect.y = rocket.rect.y
            b = 0
            patron -= 1
        if keys_pressed[K_ESCAPE]:
            pause1 = True
            pause()
            #game = False

class evil(obekt):
    def update(self): 
        if self.rect.y < 600:
            self.rect.y += self.speed
        else:
            self.rect.y = -100
            self.rect.x = randint(0,900)
            if yr == 1:
                    self.speed = randint(5,10)
            if yr == 2:
                self.speed = randint(6,10)
            if yr == 3:
                self.speed = randint(7,10)
            if yr == 4:
                self.speed = randint(8,10)
            if yr == 5:
                self.speed = randint(9,10)
            if yr == 6:
                self.speed = 10
        
font.init()
font1 = font.Font(None, 25)
font2 = font.Font(None, 50)
font3 = font.Font(None, 35)

ino = evil("ufo.png", 100,180, 5)
rocket = igrok("rocket.png", 450,520, 6)
ino2 = evil("ufo.png", 150,180, 5)
ino3 = evil("ufo.png", 250,180, 5)
ino4 = evil("ufo.png", 350,180, 5)
ino5 = evil("ufo.png", 450,180, 5)
ino6 = evil("ufo.png", 550,180, 5)
tnt = evil("tnt.png", 50,180,5)
hello = evil("hello.png", 0,180,5)
coin = evil("coin.png", randint(100,600),180,5)

inoplan = [ino,ino2,ino3,ino4,ino5,ino6]
bonus = [coin,hello,tnt]
def obrat():
    global b,n,h,patron,inoplan,game,font1,cocoin,bul,shop,endgame,ap1,ap2,o,yr
    while game:
        scr.blit(back,(0,0))
        txt = "Здоровье "+str(h)
        text = font1.render(txt, 1, (255,0,0))
        txt1 = "Очки "+str(o)
        text1 = font1.render(txt1, 1, (255,0,0))
        txtp = "Количество патронов "+str(patron)
        textp = font1.render(txtp,1,(255,0,0))
        txtc = "Количество монет "+str(cocoin)
        textc = font1.render(txtc,1,(255,0,0))
        cola1txt = "Количество аптечек по 10 жизней "+str(ap1)+"(нажмите e)"
        cola1text = font1.render(cola1txt,1,(255,0,0))
        cola2txt = "Количество аптечек по 50 жизней "+str(ap2)+"(нажмите r)"
        cola2text = font1.render(cola2txt,1,(255,0,0))
        scr.blit(cola2text, (10,120))
        scr.blit(cola1text, (10,100))
        scr.blit(text, (10,20))
        scr.blit(text1, (10,40))
        scr.blit(textp, (10,60))
        scr.blit(textc, (10,80))
        for e in event.get():
            if e.type == QUIT:
                game = False
                
        rocket.reset()
        rocket.update()
        hello.reset()
        hello.update()
        tnt.reset()
        tnt.update()
        ino.reset()
        ino.update()
        ino2.reset()
        ino2.update()
        ino3.reset()
        ino3.update()
        ino4.reset()
        ino4.update()
        ino5.reset()
        ino5.update()
        ino6.reset()
        ino6.update()
        bul.reset()
        bul.update()
        bul.fire()
        coin.reset()
        coin.update()
        keys_pressed = key.get_pressed()
        if keys_pressed[K_s]:
            game = False
            shop1()
        if keys_pressed[K_e] and ap1 >= 1:
            h += 10
            ap1 -= 1
        if keys_pressed[K_r] and ap2 >= 1:
            h += 50
            ap2 -= 1
        if keys_pressed[K_c]:
            h += 10000
            cocoin += 10000
            o += 100
        for q in inoplan:
            if sprite.collide_rect(bul,q):
                q.rect.x = randint(0,900)
                q.rect.y = -100
                bul.rect.y = -1000
                if yr == 1:
                    q.speed = randint(5,10)
                if yr == 2:
                    q.speed = randint(6,10)
                if yr == 3:
                    q.speed = randint(7,10)
                if yr == 4:
                    q.speed = randint(8,10)
                if yr == 5:
                    q.speed = randint(9,10)
                if yr == 6:
                    q.speed = 10
                b = 1
                o += 1
                
        for q in inoplan:
            if sprite.collide_rect(rocket,q):
                h -= 10
                q.rect.x = randint(0,900)
                q.rect.y = -100
        for q in inoplan:
            if sprite.collide_rect(coin,q):
                q.rect.x = randint(0,900)
                q.rect.y = -100
        if sprite.collide_rect(ino, ino2):
            ino2.rect.y = -100
            ino2.rect.x = randint(0,900)
        if sprite.collide_rect(ino, ino3):
            ino3.rect.y = -100
            ino3.rect.x = randint(0,900)
        if sprite.collide_rect(ino, ino4):
            ino4.rect.y = -100
            ino4.rect.x = randint(0,900)
        if sprite.collide_rect(ino, ino5):
            ino5.rect.y = -100
            ino5.rect.x = randint(0,900)
        if sprite.collide_rect(ino, ino6):
            ino6.rect.y = -100
            ino6.rect.x = randint(0,900)
        if sprite.collide_rect(ino, ino6):
            ino6.rect.y = -100
            ino6.rect.x = randint(0,900)
        if sprite.collide_rect(ino2, ino3):
            ino3.rect.y = -100
            ino3.rect.x = randint(0,900)
        if sprite.collide_rect(ino2, ino4):
            ino4.rect.y = -100
            ino4.rect.x = randint(0,900)
        if sprite.collide_rect(ino2, ino5):
            ino5.rect.y = -100
            ino5.rect.x = randint(0,900)
        if sprite.collide_rect(ino2, ino6):
            ino6.rect.y = -100
            ino6.rect.x = randint(0,900)
        if sprite.collide_rect(ino3, ino4):
            ino4.rect.y = -100
            ino4.rect.x = randint(0,900)
        if sprite.collide_rect(ino3, ino5):
            ino5.rect.y = -100
            ino5.rect.x = randint(0,900)
        if sprite.collide_rect(ino3, ino6):
            ino6.rect.y = -100
            ino6.rect.x = randint(0,900)
        if sprite.collide_rect(ino4, ino5):
            ino5.rect.y = -100
            ino5.rect.x = randint(0,900)
        if sprite.collide_rect(ino4, ino6):
            ino6.rect.y = -100
            ino6.rect.x = randint(0,900)
        if sprite.collide_rect(ino5, ino6):
            ino6.rect.y = -100
            ino6.rect.x = randint(0,900)
        if sprite.collide_rect(tnt, ino):
            ino.rect.y = -100
            ino.rect.x = randint(0,900)
        if sprite.collide_rect(tnt, ino2):
            ino2.rect.y = -100
            ino2.rect.x = randint(0,900)
        if sprite.collide_rect(tnt, ino3):
            ino3.rect.y = -100
            ino3.rect.x = randint(0,900)
        if sprite.collide_rect(tnt, ino4):
            ino4.rect.y = -100
            ino4.rect.x = randint(0,900)
        if sprite.collide_rect(tnt, ino5):
            ino5.rect.y = -100
            ino5.rect.x = randint(0,900)
        if sprite.collide_rect(tnt, ino6):
            ino6.rect.y = -100
            ino6.rect.x = randint(0,900)
        if sprite.collide_rect(hello, ino):
            ino.rect.y = -100
            ino.rect.x = randint(0,900)
        if sprite.collide_rect(hello, ino2):
            ino2.rect.y = -100
            ino2.rect.x = randint(0,900)
        if sprite.collide_rect(hello, ino3):
            ino3.rect.y = -100
            ino3.rect.x = randint(0,900)
        if sprite.collide_rect(hello, ino4):
            ino4.rect.y = -100
            ino4.rect.x = randint(0,900)
        if sprite.collide_rect(hello, ino5):
            ino5.rect.y = -100
            ino5.rect.x = randint(0,900)
        if sprite.collide_rect(hello, ino6):
            ino6.rect.y = -100
            ino6.rect.x = randint(0,900)
        if sprite.collide_rect(hello, tnt):
            hello.rect.y = -100
            hello.rect.x = randint(0,900)
        if sprite.collide_rect(hello, coin):
            hello.rect.y = -100
            hello.rect.x = randint(0,900)
        if sprite.collide_rect(coin, tnt):
            coin.rect.y = -100
            coin.rect.x = randint(0,900)
        if sprite.collide_rect(bul, tnt):
            ino.rect.x = randint(0,900)
            ino.rect.y = -100
            ino2.rect.x = randint(0,900)
            ino2.rect.y = -100
            ino3.rect.x = randint(0,900)
            ino3.rect.y = -100
            ino4.rect.x = randint(0,900)
            ino4.rect.y = -100
            ino5.rect.x = randint(0,900)
            ino5.rect.y = -100
            ino6.rect.x = randint(0,900)
            ino6.rect.y = -100
            bul.rect.y = -1000
            if yr <= 5:
                coin.rect.y = -1200
                coin.rect.x = randint(0,900)
            if yr == 6:
                coin.rect.y = -1400
                coin.rect.x = randint(0,900)
            if yr <= 5:
                hello.rect.y = -1200
                hello.rect.x = randint(0,900)
            if yr == 6:
                hello.rect.y = -1400
                hello.rect.x = randint(0,900)
            if yr <= 5:
                tnt.rect.y = -1200
                tnt.rect.x = randint(0,900)
            if yr == 6:
                tnt.rect.y = -1400
                tnt.rect.x = randint(0,900)
            
        if sprite.collide_rect(rocket, tnt):
            tnt.rect.x = randint(0,900)
            tnt.rect.y = -100
            h -= 20
        if sprite.collide_rect(bul, hello):
            hello.rect.y = -1200
            hello.rect.x = randint(0,900)
            bul.rect.y = -1000
        if sprite.collide_rect(rocket, hello):
            h += 5
            hello.rect.y = -1200
            hello.rect.x = randint(0,900)
            bul.rect.y = -1000
        if sprite.collide_rect(rocket,coin):
            coin.rect.y = -1100
            coin.rect.x = randint(0,900)
            cocoin += 1
        if sprite.collide_rect(bul,coin):
            bul.rect.y = -1000
            b=1
            coin.rect.y = -1100
            coin.rect.x = randint(0,900)
        if h <= 0:
            game = False
            game2 = True
            endgame()
            font1 = font.Font(None, 100)
        if o >= 15:
            yr = 2
        if o >= 30:
            yr = 3
        if o >= 50:
            yr = 4
        if o >= 70:
            yr = 5
        if o >= 100:
            yr = 6
        clock.tick(FPS)
        display.update()

obrat()
