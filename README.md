# Water-level-monitoring-source-code

sonar sender:

#include <hcsr04.h>

#define TRIG_PIN 12
#define ECHO_PIN 13

HCSR04 hcsr04(TRIG_PIN, ECHO_PIN, 20, 4000);

void setup() {
  Serial.begin(9600);
  }
  

void loop() {
  Serial.print(hcsr04.distanceInMillimeters());
  Serial.print(";");
  
  delay(250);
}

Sonar Reiciever:

#include <LiquidCrystal.h>
#include <SoftwareSerial.h>

SoftwareSerial mySerial(10, 11); // RX,TX

const int rs = 2, e = 3, d4 = 4, d5 = 5, d6 = 6, d7 = 7;
LiquidCrystal lcd(rs, e, d4, d5, d6, d7);

void setup() {
  Serial.begin(9600);
  lcd.begin(16, 2);
  pinMode(9,OUTPUT);
  pinMode(8,OUTPUT);
  pinMode(12,OUTPUT);
  mySerial.begin(9600);
  }
  

void loop() {
  while (Serial.available()){
  lcd.home();
  lcd.clear();
  int mm = Serial.readStringUntil(59).toInt();
  lcd.print(mm / 1000.0);
  lcd.print("Meters");
   if(mm/1000.0<.11){
    digitalWrite(9,HIGH);
    digitalWrite(8,LOW);
    digitalWrite(12,LOW);  
  }
   if(mm/1000.0<.05){
    digitalWrite(9,HIGH);
    digitalWrite(8,HIGH);
    digitalWrite(12,HIGH);
  }
  mySerial.println(mm/1000.0);
  mySerial.println("Meters");
  }
  }
  
