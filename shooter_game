from pygame import *
from random import randint

font.init()

number = 0

score = 0

class Bullet(sprite.Sprite):
    def __init__(self, player_x, player_y, bullet_speed):
        super().__init__()
        self.image = transform.scale(image.load("bullet.png"), (10, 20))
        self.rect = self.image.get_rect()
        self.rect.x = player_x + 27 
        self.rect.y = player_y
        self.speed = bullet_speed

    def update(self):
        self.rect.y -= self.speed
        if self.rect.y < 0:
            self.kill()

class GameSprite(sprite.Sprite):
    def __init__(self, player_image, player_speed, player_x, player_y):
        super().__init__()
        self.image = transform.scale(image.load(player_image), (65, 65))
        self.speed = player_speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y

    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

class Player(GameSprite):
    bullet_speed = 5
    bullets = sprite.Group()

    def update(self):
        keys_pressed = key.get_pressed()
        if (keys_pressed[K_LEFT] or keys_pressed[K_a]) and self.rect.x > 5:
            self.rect.x -= self.speed
        if (keys_pressed[K_RIGHT] or keys_pressed[K_d]) and self.rect.x < win_width - 65:
            self.rect.x += self.speed

    def shoot(self):
        bullet = Bullet(self.rect.x, self.rect.y, self.bullet_speed)
        self.bullets.add(bullet)

    def reset(self):
        super().reset()
        self.bullets.draw(window)

class Enemy(GameSprite):
    def update(self):
        global number
        self.rect.y += self.speed
        if self.rect.y > win_height:
            self.rect.y = 0
            self.rect.x = randint(0, win_width - 65)
            number += 1  
            
win_width = 700
win_height = 500
window = display.set_mode((win_width, win_height))
display.set_caption('Шутер')
background = transform.scale(image.load('galaxy.jpg'), (win_width, win_height))
player = Player('rocket.png', 5, win_width // 2 - 32, win_height - 70)

monsters = sprite.Group()
for i in range(1, 11):
    monster = Enemy('ufo.png', randint(1, 2), randint(0, win_width - 65), 0)
    monsters.add(monster)
asteroids = sprite.Group()
for i in range(1, 4):
    asteroid = Enemy('asteroid.png', randint(1,2), randint(0, win_width-65),0)
    asteroids.add(asteroid)
font = font.SysFont(None, 35)
game = True
finish = False
clock = time.Clock()
health = 3
while game:
    for e in event.get():
        if e.type == QUIT:
            game = False
        if e.type == KEYDOWN and e.key == K_SPACE:
            player.shoot()
    if finish != True:
        window.blit(background, (0, 0))

        player.update()
        player.reset()

        monsters.update()
        monsters.draw(window)
         
        asteroids.update()
        asteroids.draw(window)

        player.bullets.update()
        for bullet in player.bullets:
            hits = sprite.spritecollide(bullet, monsters, True)
            if hits:
                bullet.kill()
                score += len(hits)
        
        passed_text = font.render(f'Пропущено: {number}', True, (255, 255, 255))
        score_text = font.render(f'Счёт: {score}', True, (255, 255, 255))
        window.blit(passed_text, (10, 30))
        window.blit(score_text, (10, 5))
        
        lose = font.render('YOU LOSE!', True, (255,0,0))
        win = font.render('YOU WIN!', True, (0, 255, 0))
        
        asteroid_hit = sprite.spritecollide(player, asteroids, False)
        if asteroid_hit:
            health -= 1
            asteroids.remove(asteroid)
        health_text = font.render(f'Здоровье: {health}', True, (255,255,255))
        window.blit(health_text, (500, 30))
        playercollide = sprite.spritecollide(player, monsters, False)
        if number > 2 or playercollide or health < 1:
            window.blit(lose, (300, 230))
            finish = True
        if score > 9:
            window.blit(win, (300, 230))
            finish = True
    clock.tick(60)
    display.update()
