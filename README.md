# SEMI-AUTOMATED-PROJECT
import RPi.GPIO as GPIO
import time
from twilio.rest import Client

# Twilio credentials
account_sid = 'YourTwilioAccountSID'
auth_token = 'YourTwilioAuthToken'
from_number = '+1234567890'  
to_number = '+1234567890'      

# GPIO setup
FLAME_SENSOR_PIN = 17  
SMOKE_SENSOR_PIN = 27   
LED_PIN = 22            
SERVO1_PIN = 23         
SERVO2_PIN = 24        

# Setup GPIO
GPIO.setmode(GPIO.BCM)
GPIO.setup(FLAME_SENSOR_PIN, GPIO.IN)
GPIO.setup(SMOKE_SENSOR_PIN, GPIO.IN)
GPIO.setup(LED_PIN, GPIO.OUT)

# Function to send SMS using Twilio
def send_sms(message):
    client = Client(account_sid, auth_token)
    client.messages.create(
        body=message,
        from_=from_number,
        to=to_number
    )
    print("SMS sent:", message)

# Function to control the servo
def control_servo(pin, angle):
    # Here you'd control the servo motor; this is a placeholder
    # You can use a library like RPi.GPIO or any other servo control library.
    print(f"Controlling Servo on pin {pin} to angle {angle}")

try:
    while True:
        flame_detected = GPIO.input(FLAME_SENSOR_PIN)
        smoke_detected = GPIO.input(SMOKE_SENSOR_PIN)

        if flame_detected:
            GPIO.output(LED_PIN, GPIO.HIGH)
            send_sms("Fire Detected!")
            control_servo(SERVO1_PIN, 90)  # Example: Rotate Servo 1 to 90 degrees
        elif smoke_detected:
            GPIO.output(LED_PIN, GPIO.HIGH)
            send_sms("Smoke Detected!")
            control_servo(SERVO2_PIN, 90)  # Example: Rotate Servo 2 to 90 degrees
        else:
            GPIO.output(LED_PIN, GPIO.LOW)

        time.sleep(1)

except KeyboardInterrupt:
    print("Exiting program...")

finally:
    GPIO.cleanup()

