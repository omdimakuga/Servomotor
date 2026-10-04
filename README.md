#include <Arduino.h>
#include <Servo.h>

const uint8_t JOYSTICK_X_PIN = A0;
const uint8_t JOYSTICK_Y_PIN = A1;
const uint8_t SERVO_PIN = 9;

const int CENTER_DEADBAND = 35;

Servo controlledServo;
int lastAngle = -1;

void setup() {
  pinMode(JOYSTICK_X_PIN, INPUT);
  pinMode(JOYSTICK_Y_PIN, INPUT);

  controlledServo.attach(SERVO_PIN);
  controlledServo.write(90);
  lastAngle = 90;
}

void loop() {
  int xValue = analogRead(JOYSTICK_X_PIN);
  int angle = map(xValue, 0, 1023, 0, 180);

  // Keep the servo still when the joystick is released near its centre.
  if (abs(xValue - 512) <= CENTER_DEADBAND) {
    angle = 90;
  }

  if (angle != lastAngle) {
    controlledServo.write(angle);
    lastAngle = angle;
  }

  delay(20);
}
