# InstaFolio-AI-Image-Viewer
"""
Sanyerlis Camacaro - CCSC285 - Sancamac@uat.edu Assignment:
Assignment 1.1: AI Image Viewer

"InstaFolio"

This code demonstrates how to:

Display a Single Image.
Create a Pygame window that is the same size as the image.
Load an image and draw it on the Pygame window.
Creating an Image List.
Write a function that gets the names of all images in a directory. Make sure to include error handling for situations where the directory doesn't exist
or doesn't contain any images.
Create a list of Pygame images by loading each image file in the directory.
Creating the Viewer: Now let's make it possible to view all the images.
Create a Pygame loop that displays each image in the list for a set amount of time (like a slideshow) AND / OR
Make sure to include controls for manually switching between images. Use Pygame's event system to listen for key presses (such as the arrow keys) 
to move to the next or previous image.
(Optional) Enhancing the Viewer: Let's add some more features to our viewer. Display the name of the current image on the Pygame window. 
You might want to look into Pygame's font module for this.
"""
# This will be an Image viewer application called InstaFolio

# ************************Libraries*****************************
# Add our libraries
# Pygame library to allow us to make a GUI
import pygame
# OS library to allow us to interact with the operating system
import os

# ************************Pygame Object*****************************
# Initialize pygame - Create an instance of Pygame
pygame.init()

# ************************Window Init*****************************
# Create constants for the screen width and height
WINDOW_WIDTH = 960
WINDOW_HEIGHT = 1000
# Create a window object
window = pygame.display.set_mode((WINDOW_WIDTH, WINDOW_HEIGHT))
# Set the title of the window
pygame.display.set_caption("InstaFolio")

# *************************Files****************************
# Set the path to our images folder, this will be a constant
IMAGE_DIRECTORY = "images"
# Get a list of all the files in the images folder
image_filenames = [filename for filename in os.listdir(IMAGE_DIRECTORY) if filename.endswith(".jpeg") or filename.endswith(".PNG") or filename.endswith(".gif") or filename.endswith(".jpeg") or filename.endswith("jfif")]

# ****************** Load each image into a list we can loop through **********************
# Create an empty list for our images
images = []
# Loop through each image filename
for filename in image_filenames:
    # Get the full path to the image
    image_path = os.path.join(IMAGE_DIRECTORY, filename)
    # Load the image into memory
    image = pygame.image.load(image_path)
    # Add the image to our list of images
    images.append(image)

# ************************* Font Setup ****************************
# Import the font module
import pygame.font

# Create a font object
font = pygame.font.Font(None, 36)  # You can adjust the font size as needed

# ******************************** Main Program Loop - Display Images *******************************************
# Create a variable to hold the index of the current image
current_image_index = 0
# Create a variable to hold the current image from the image list
current_image = images[current_image_index]
# Create a clock object to control the frame rate
clock = pygame.time.Clock()
# Create and start our main program loop
# This bool will keep the window open until we set it to false
is_running = True
# Loop until running is set to false
while is_running:
    # Clear the screen
    window.fill((255, 255, 255))
    
    # Display the current image
    window.blit(current_image, (0, 0))
    
    # Render the image name as text
    image_name = image_filenames[current_image_index]
    text = font.render(image_name, True, (0, 0, 0))  # Text color: black
    
    # Blit the text onto the window
    window.blit(text, (10, 10))  # Adjust the position as needed
    
    # Update the display
    pygame.display.flip()
    
    # Handle any events
    for event in pygame.event.get():
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_RIGHT:
                current_image_index = (current_image_index + 1) % len(images)
                current_image = images[current_image_index]
            elif event.key == pygame.K_LEFT:
                current_image_index = (current_image_index - 1) % len(images)
                current_image = images[current_image_index]
            elif event.key == pygame.K_ESCAPE:
                is_running = False

    # Set the frame rate to 30 frames per second
    clock.tick(30)

# ******************************* End of program ******************************
# Quit the application
pygame.quit()
