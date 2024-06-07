import pygame
import sys

# Initialize pygame
pygame.init()

# Set up screen dimensions
SCREEN_WIDTH = 400
SCREEN_HEIGHT = 600
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Floppy Bird")

# Set up colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Set up game variables
bird_x = 50
bird_y = 250
bird_velocity = 0
gravity = 0.25
jump_strength = -5
game_over = False

# Set up game objects
bird_image = pygame.image.load("bird.png")
bird_rect = bird_image.get_rect(center=(bird_x, bird_y))
pipe_image = pygame.image.load("pipe.png")
pipe_rect = pipe_image.get_rect(topleft=(300, 250))

# Main game loop
while True:
    # Event handling
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_SPACE and not game_over:
                bird_velocity = jump_strength

    # Update bird position
    bird_velocity += gravity
    bird_y += bird_velocity
    bird_rect.centery = bird_y

    # Update pipe position
    pipe_rect.x -= 2

    # Check for collision with pipe
    if bird_rect.colliderect(pipe_rect):
        game_over = True

    # Check for game over conditions
    if bird_rect.top <= 0 or bird_rect.bottom >= SCREEN_HEIGHT:
        game_over = True

    # Draw everything
    screen.fill(WHITE)
    screen.blit(bird_image, bird_rect)
    screen.blit(pipe_image, pipe_rect)
    pygame.display.flip()

    # Check for game over
    if game_over:
        pygame.time.wait(2000)  # Wait for 2 seconds
        bird_x = 50
        bird_y = 250
        bird_velocity = 0
        pipe_rect.left = 300
        game_over = False
