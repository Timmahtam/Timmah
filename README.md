import math
import numpy as np

class StepTracker:
    def __init__(self, threshold=1.2, window_size=10):
        self.threshold = threshold
        self.window_size = window_size
        self.acceleration_data = []
        self.steps = 0
    
    def track_step(self, acceleration):
        self.acceleration_data.append(acceleration)
        if len(self.acceleration_data) > self.window_size:
            self.acceleration_data.pop(0)
        if len(self.acceleration_data) == self.window_size:
            if self.is_step_detected():
                self.steps += 1
    
    def is_step_detected(self):
        acceleration_vector = np.array(self.acceleration_data)
        magnitude = np.linalg.norm(acceleration_vector)
        avg_magnitude = magnitude / self.window_size
        return avg_magnitude > self.threshold


# Example usage:
step_tracker = StepTracker()

# Simulating accelerometer data (acceleration along the vertical axis)
accelerometer_data = [0.9, 0.8, 1.1, 1.3, 1.2, 0.9, 0.7, 1.2, 1.5, 1.4, 0.8, 0.6]

for acceleration in accelerometer_data:
    step_tracker.track_step(acceleration)

print("Total Steps:", step_tracker.steps)
