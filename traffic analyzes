import cv2
import torch
import numpy as np
import paho.mqtt.client as mqtt
import time

# Configuration
ESP32_CAM_URL = "http://<ESP32_CAM_IP>/stream"
MQTT_BROKER = 'broker.hivemq.com'
MQTT_TOPIC = 'lumiflow/traffic'

# Load YOLOv5 Model
model = torch.hub.load('ultralytics/yolov5', 'yolov5s')
model.classes = [2, 3, 5, 7]  # Only detect cars, motorcycles, buses, trucks

# Initialize MQTT Client
client = mqtt.Client("Traffic_Analyzer")
client.connect(MQTT_BROKER)

# Start Video Capture
cap = cv2.VideoCapture(ESP32_CAM_URL)

def calculate_traffic_density(frame, detections):
    frame_area = frame.shape[0] * frame.shape[1]
    total_vehicle_area = 0
    
    for det in detections:
        x1, y1, x2, y2 = map(int, det[:4])
        vehicle_area = (x2 - x1) * (y2 - y1)
        total_vehicle_area += vehicle_area
    
    traffic_density = total_vehicle_area / frame_area
    return traffic_density

while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
        print("Failed to capture frame")
        break
    
    # Inference
    results = model(frame)
    detections = results.xyxy[0].cpu().numpy()
    
    # Draw Bounding Boxes
    for det in detections:
        x1, y1, x2, y2, conf, cls = map(int, det)
        cv2.rectangle(frame, (x1, y1), (x2, y2), (0, 255, 0), 2)
        cv2.putText(frame, model.names[cls], (x1, y1 - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.6, (0, 255, 0), 2)
    
    # Calculate Traffic Density
    traffic_density = calculate_traffic_density(frame, detections)
    print(f"Traffic Density: {traffic_density}")
    
    # Send Data to MQTT Broker
    client.publish(MQTT_TOPIC, str(traffic_density))
    
    # Display Frame
    cv2.imshow('Traffic Analysis', frame)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
