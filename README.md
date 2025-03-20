# Design-and-Implementation-of-Smart-Zebra-Crossing-using-Arduino

#include<LiquidCrystal.h> 
// Define the pins for the ultrasonic sensor 
const int trigPin = 7; 
const int echoPin = 8; 
int ledr=A0; 
int ledg=A1; 
// Variables for storing distance measurement 
long duration; 
int distance; 
#include <Servo.h> 
// Create a Servo object 
Servo myServo; 
const int rs = 12, en = 11, d4 = 5, d5 = 4, d6 = 3, d7 = 2, ct=9; 
LiquidCrystal mylcd(rs, en, d4, d5, d6, d7); 
void setup() { 
analogWrite(ct,100); 
mylcd.begin(16, 2); 
// Attach the servo on pin 9 
myServo.attach(10); 
Serial.begin(9600); 
// Define the trigPin and echoPin as OUTPUT and INPUT 
pinMode(trigPin, OUTPUT); 
pinMode(echoPin, INPUT); 
pinMode(A0, OUTPUT); 
pinMode(A1, OUTPUT); 
} 
void loop() { 
// Clear the trigPin 
digitalWrite(trigPin, LOW); 
delayMicroseconds(2); 
// Set the trigPin on HIGH state for 10 microseconds 
digitalWrite(trigPin, HIGH); 
delayMicroseconds(10); 
digitalWrite(trigPin, LOW); 
// Read the echoPin, which measures the time duration of the sound wave 
duration = pulseIn(echoPin, HIGH); 
// Calculate the distance (in cm) based on the speed of sound 
distance = duration * 0.034 / 2; 
// Print the distance to the Serial Monitor 
Serial.print("Distance: "); 
Serial.print(distance); 
Serial.println(" cm"); 
// Delay before next measurement 
delay(500); 
if(distance<=15) 
{ 
mylcd.setCursor(0, 0); 
mylcd.print("Pedestrian"); 
mylcd.setCursor(0, 1); 
mylcd.print("crossing STOP"); 
digitalWrite(A0,LOW); 
digitalWrite(A1,HIGH); 
myServo.write(120); 
delay(500); // Delay for 1 second 
//  myServo.write(0); 
//   delay(5000); // Delay for 1 second 
} 
else 
{ 
mylcd.setCursor(0, 0); 
mylcd.print("No Pedestrian"); 
mylcd.setCursor(0, 1); 
mylcd.print("Crossing Move"); 
digitalWrite(A0,HIGH); 
digitalWrite(A1,LOW); 
myServo.write(0); 
delay(500); 
} 
}  
