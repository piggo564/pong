# pong
# My pong game made with pygame

import pygame
pygame.init()

clock = pygame.time.Clock()

win_height, win_width = 400, 800

win = pygame.display.set_mode((win_width, win_height))
pygame.display.set_caption("PONG -By George")


# Paddle
paddle_width, paddle_height = 20, 100
paddle_vel = 5


# Paddle 1
paddle1_x = int((win_width / 10) - paddle_width)
paddle1_y = int((win_height / 2) - (paddle_height / 2))
paddle1_score = 0


# Paddle 2
paddle2_x = int(win_width - (win_width / 10))
paddle2_y = paddle1_y
paddle2_score = 0


# Ball
ball_radius = 10
ball_diameter = ball_radius * 2
ball_x = int((win_width / 2) - (ball_radius / 2))
ball_y = int((win_height / 2) - (ball_radius / 2))
ball_vel_x, ball_vel_y = 7, 7


# Score Text
textFont = pygame.font.SysFont('Courier', 30)
text_x = int(win_width / 5)
text_y = int(win_width / 50)


# Draws everything 
def redraw():
    win.fill((0,0,0))

    textSurface = textFont.render('PLAYER 1: {}   PLAYER 2: {} '.format(paddle1_score, paddle2_score), True, (255, 255, 255))
    win.blit(textSurface, (text_x, text_y))
    
    pygame.draw.rect(win, (255, 255, 255), (paddle1_x, paddle1_y, paddle_width, paddle_height))
    pygame.draw.rect(win, (255, 255, 255), (paddle2_x, paddle2_y, paddle_width, paddle_height))
    pygame.draw.circle(win, (255, 255, 255), (ball_x, ball_y), ball_radius)
    pygame.display.update()
    

# GAME LOOP
run = True
while run:
    clock.tick(50)

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            run = False

    ball_x += ball_vel_x
    ball_y += ball_vel_y

    # Ball hitting edge of window
    if ball_x < 0:
        ball_x = int((win_width / 2) - (ball_radius / 2))
        ball_vel_x *= -1
        paddle2_score += 1
    elif ball_x + ball_diameter > win_width:
        ball_x = int((win_width / 2) - (ball_radius / 2))
        ball_vel_x *= -1
        paddle1_score += 1
    
    if ball_y < 0:
        ball_vel_y *= -1
    elif ball_y + ball_diameter > win_height:
        ball_vel_y *= -1


    # Ball hitting paddle
    if paddle1_y < ball_y < (paddle1_y + paddle_height) and paddle1_x < ball_x < (paddle1_x + paddle_width):
        ball_vel_x *= -1
        ball_x = paddle1_x + paddle_width
        
    if paddle2_y < ball_y < (paddle2_y + paddle_height) and paddle2_x + paddle_width > ball_x > paddle2_x:
        ball_vel_x *= -1
        ball_x = paddle2_x - ball_diameter


    # User keyboard input
    keys = pygame.key.get_pressed()

    if keys[pygame.K_w] and paddle1_y > 0:
        paddle1_y -= paddle_vel

    if keys[pygame.K_s] and paddle1_y + paddle_height < win_height:
        paddle1_y += paddle_vel

    if keys[pygame.K_UP] and paddle2_y > 0:
        paddle2_y -= paddle_vel

    if keys[pygame.K_DOWN] and paddle2_y + paddle_height < win_height:
        paddle2_y += paddle_vel

    redraw()


pygame.quit()
