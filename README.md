import pygame
import random

# Initialize Pygame
pygame.init()

# Set the window size
window_size = (640, 480)

# Create the window
screen = pygame.display.set_mode(window_size)

# Set the title of the window
pygame.display.set_caption("Noah's Ark")

# Load the images
noah_image = pygame.image.load("noah.png")
ark_image = pygame.image.load("ark.png")
animal_images = ["lion.png", "tiger.png", "giraffe.png", "elephant.png"]
animal_images = [pygame.image.load(image) for image in animal_images]

# Set the starting positions
noah_x = window_size[0] // 2
noah_y = window_size[1] - noah_image.get_height()
ark_x = window_size[0] // 2
ark_y = noah_y - ark_image.get_height()
animals = []

# Set the movement speed
speed = 5

# Run the game loop
running = True
while running:
    # Check for events
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Get the keys that are being pressed
    keys = pygame.key.get_pressed()

    # Move Noah left or right
    if keys[pygame.K_LEFT]:
        noah_x -= speed
    elif keys[pygame.K_RIGHT]:
        noah_x += speed

    # Keep Noah within the screen bounds
    if noah_x < 0:
        noah_x = 0
    elif noah_x + noah_image.get_width() > window_size[0]:
        noah_x = window_size[0] - noah_image.get_width()

    # Make new animals fall from the sky
    if random.randint(0, 100) == 0:
        x = random.randint(0, window_size[0] - animal_images[0].get_width())
        y = 0
        image = random.choice(animal_images)
        animals.append((x, y, image))

    # Move the animals down
    for i in range(len(animals)):
        x, y, image = animals[i]
        y += speed
        animals[i] = (x, y, image)

    # Remove animals that have reached the ground
    animals = [animal for animal in animals if animal[1] < window_size[1]]

    # Update the game state

    # Draw the game
    screen.fill((255, 255, 255))  # Clear the screen
    screen.blit(ark_image, (ark_x, ark_y))  # Draw the ark
    screen.blit(noah_image, (noah_x, noah_y))  # Draw Noah
    for x, y, image in animals:  # Draw the animals
        screen.blit(image,
