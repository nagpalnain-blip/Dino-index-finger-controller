# Dino Index Finger Controller
This project lets you control the Dino game using real-time Digital Image Processing, through your own webcam!

## How it works-
* It uses OpenCV and MediaPipe to detect your hand and simulates a jump when you raise your index finger above the screen’s midline.
* When you raise your index finger above the red line shown on the screen, the program does the work of pressing the spacebar to make the Dino jump.
* The red line in the middle of the screen serves as the jump threshold.
* You can quit the program anytime by pressing the ‘q’ key on your keyboard.

## Downloads needed-
1. Make sure you have Python installed on your system.
2. Install the required dependencies by running:
   pip install opencv-python mediapipe pyautogui
   
## Keep in mind-
* Make sure your camera's red line aligns with the groundline in the dino game.
* The default cooldown between jumps is 0.5 seconds, but you can customize this in the code.
