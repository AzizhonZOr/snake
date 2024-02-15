# snake
import pygame
import sys
import random

pygame.init()
windows = pygame.display.set_mode((640, 480))
pygame.display.set_caption("Iloncha o'yini")

# o'yin parametrlari
snake_position = [100, 50]
snake_body = [[100, 50], [90, 50], [80, 50], [70, 50]]
food_position = [random.randrange(1, (640 // 10)) * 10,
                 random.randrange(1, (480 // 10)) * 10]
food_spawn = True
direction = "RIGHT"
change_to = direction
speed = 10
running = True

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        # Boshqaruv
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_UP:
                change_to = "UP"
            if event.key == pygame.K_DOWN:
                change_to = "DOWN"
            if event.key == pygame.K_LEFT:
                change_to = "LEFT"
            if event.key == pygame.K_RIGHT:
                change_to = "RIGHT"

    # yo'nalishni aniqlash
    if change_to == "UP" and direction != "DOWN":
        direction = "UP"
    if change_to == "DOWN" and direction != "UP":
        direction = "DOWN"
    if change_to == "LEFT" and direction != "RIGHT":
        direction = "LEFT"
    if change_to == "RIGHT" and direction != "LEFT":
        direction = "RIGHT"

    # ilonchani harakatlantirish
    if direction == "UP":
        snake_position[1] -= 10
    if direction == "DOWN":
        snake_position[1] += 10
    if direction == "LEFT":
        snake_position[0] -= 10
    if direction == "RIGHT":
        snake_position[0] += 10

    # ilonchani tanasini yangilash
    snake_body.insert(0, list(snake_position))
    if snake_position == food_position:
        food_spawn = False
    else:
        snake_body.pop()


    # Ovqatni qayta joylashtirish
    if not food_spawn:
        food_position = [random.randrange(1, (640 // 10)) * 10,
                         random.randrange(1, (480 // 10)) * 10]
    food_spawn = True

    windows.fill("black")

    # ilon va ovqatni chizish
    for position in snake_body:
        pygame.draw.rect(windows, "green", pygame.Rect(position[0], position[1], 10, 10))
    pygame.draw.rect(windows, "red", pygame.Rect(food_position[0], food_position[1], 10, 10))

    # ilonchani o'zi va devorga urilishini tekshirish
    if snake_position[0] < 0 or snake_position[0] > 640-10:
        running = False
        print("You Lose")
    if snake_position[1] < 0 or snake_position[1] > 480 - 10:
        running = False
        print("You Lose")
    for block in snake_body[1:0]:
        if snake_position == block:
            running = False
        print("You Lose")

    pygame.display.flip()
    pygame.time.Clock().tick(speed)
