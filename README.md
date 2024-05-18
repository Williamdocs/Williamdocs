import pygame
import random

# Initialize pygame
pygame.init()

# Screen dimensions
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
BRICK_WIDTH = 60
BRICK_HEIGHT = 30
BRICK_ROWS = 6
BRICK_COLUMNS = 13

# Colors
BLACK = (0, 0, 0)
WHITE = (255, 255, 255)
RED = (255, 0, 0)
ORANGE = (255, 165, 0)
YELLOW = (255, 255, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)

# Screen setup
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Breakout")

# Paddle
paddle = pygame.Rect(SCREEN_WIDTH // 2 - 50, SCREEN_HEIGHT - 20, 100, 10)
paddle_speed = 10

# Ball
ball = pygame.Rect(SCREEN_WIDTH // 2 - 10, SCREEN_HEIGHT // 2 - 10, 20, 20)
ball_speed = [5, -5]

# Bricks
bricks = []
colors = [RED, ORANGE, YELLOW, GREEN, BLUE]
for row in range(BRICK_ROWS):
    for col in range(BRICK_COLUMNS):
        brick = pygame.Rect(col * (BRICK_WIDTH + 5) + 35, row * (BRICK_HEIGHT + 5) + 50, BRICK_WIDTH, BRICK_HEIGHT)
        bricks.append((brick, colors[row % len(colors)]))

# Game loop
running = True
clock = pygame.time.Clock()

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Move paddle
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and paddle.left > 0:
        paddle.left -= paddle_speed
    if keys[pygame.K_RIGHT] and paddle.right < SCREEN_WIDTH:
        paddle.right += paddle_speed

    # Move ball
    ball.left += ball_speed[0]
    ball.top += ball_speed[1]

    if ball.left <= 0 or ball.right >= SCREEN_WIDTH:
        ball_speed[0] = -ball_speed[0]
    if ball.top <= 0:
        ball_speed[1] = -ball_speed[1]
    if ball.colliderect(paddle):
        ball_speed[1] = -ball_speed[1]

    # Check for brick collision
    for brick, color in bricks[:]:
        if ball.colliderect(brick):
            ball_speed[1] = -ball_speed[1]
            bricks.remove((brick, color))
            break

    if ball.bottom >= SCREEN_HEIGHT:
        running = False

    # Drawing
    screen.fill(BLACK)
    pygame.draw.rect(screen, WHITE, paddle)
    pygame.draw.ellipse(screen, WHITE, ball)
    for brick, color in bricks:
        pygame.draw.rect(screen, color, brick)

    pygame.display.flip()
    clock.tick(60)

pygame.quit()
