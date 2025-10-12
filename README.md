# Dino-index-finger-controller
#You can control the Dino in game, using your Index finger, using OpenCv and Mediapipe
import cv2
import mediapipe as mp
import pyautogui
import time

# ===== Dino Hand Tracker =====
# Raise your hand above the midline → Dino jumps
# Press 'q' to quit safely

jump_cooldown = 0.5  # seconds between jumps
last_jump_time = 0

# Initialize MediaPipe Hand detection
mp_hands = mp.solutions.hands
hands = mp_hands.Hands(min_detection_confidence=0.5, min_tracking_confidence=0.5)

# Open webcam
cap = cv2.VideoCapture(0)

# Delay to give time to switch to Chrome Dino window
time.sleep(5)
print("Starting in 5 seconds... Focus the Chrome Dino game window!")

while True:
    ret, frame = cap.read()
    if not ret:
        break

    # Mirror the image for natural movement
    frame = cv2.flip(frame, 1)
    h, w, _ = frame.shape

    # Convert frame to RGB for MediaPipe
    rgb = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
    results = hands.process(rgb)

    # If hand detected
    if results.multi_hand_landmarks:
        for hand_landmarks in results.multi_hand_landmarks:
            # Index fingertip landmark (8)
            index_tip = hand_landmarks.landmark[8]
            x, y = int(index_tip.x * w), int(index_tip.y * h)

            # Draw fingertip on screen
            cv2.circle(frame, (x, y), 10, (0, 0, 255), -1)

            # Jump condition: hand above half screen and cooldown passed
            if y < h / 2 and (time.time() - last_jump_time) > jump_cooldown:
                pyautogui.press("space")  # Simulate spacebar
                last_jump_time = time.time()
                cv2.putText(frame, "Jump!", (50, 50),
                            cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 0, 255), 2)

    # Draw threshold line
    cv2.line(frame, (0, h // 2), (w, h // 2), (0, 0, 255), 2)
    cv2.putText(frame, "Raise hand above red line to jump", (10, h - 20),
                cv2.FONT_HERSHEY_SIMPLEX, 0.7, (255, 255, 255), 2)

    cv2.imshow("Dino Hand Tracker", frame)

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# Cleanup
cap.release()
cv2.destroyAllWindows()
