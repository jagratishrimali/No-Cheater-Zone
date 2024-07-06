# No-Cheater-Zone
 "Python script for exam proctoring using OpenCV and pynput").
# no_cheater_zone.py
import cv2
import numpy as np
from pynput import keyboard, mouse

# Function to detect faces using OpenCV
def detect_faces(frame):
    face_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')
    gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    faces = face_cascade.detectMultiScale(gray, scaleFactor=1.3, minNeighbors=5)
    return faces

# Function to monitor keyboard events
def on_key_press(key):
    print(f'Key {key} pressed')

# Function to monitor mouse events
def on_move(x, y):
    print(f'Mouse moved to ({x}, {y})')

# Capture video from webcam
cap = cv2.VideoCapture(0)

# Initialize keyboard and mouse listeners
keyboard_listener = keyboard.Listener(on_press=on_key_press)
mouse_listener = mouse.Listener(on_move=on_move)

keyboard_listener.start()
mouse_listener.start()

while True:
    ret, frame = cap.read()

    # Detect faces
    faces = detect_faces(frame)

    for (x, y, w, h) in faces:
        cv2.rectangle(frame, (x, y), (x + w, y + h), (255, 0, 0), 2)

    cv2.imshow('No Cheater Zone - Exam Proctoring', frame)

    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
