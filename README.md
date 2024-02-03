### IOT Smart Irrigation System
#### https://www.tinkercad.com/things/lAgalfk7Xmd-automatic-irrigation-system-eiot-mini-project/editel?sharecode=wdmGt_pndk-74B0ZvI39YfG_eb8ORtflgbRa_WlcDUk

#include <LiquidCrystal.h>

#include <Adafruit_NeoPixel.h>
#ifdef __AVR__
#include <avr/power.h> // Required for 16 MHz Adafruit Trinket
#endif
#define NUMPIXELS 16
#define DELAYVAL 1
#define echoPin 10
#define trigPin 8
#define PIN 1
Adafruit_NeoPixel pixels(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);

const int SMP = A1;
const int TMP = A0;
const int motor = 13;
const int LedRed = 12;
const int LedGreen = 11;
//const int SON = 10;
const int peizo = 9;

LiquidCrystal lcd(2, 3, 4, 5, 6, 7);

void setup() {
  pixels.begin();
  Serial.begin(9600);
  lcd.begin(16, 2);
  lcd.print("Automated Plant");
  lcd.setCursor(0,1);
  lcd.print("Watering System!");
  delay(1000);
  pinMode(SMP, INPUT);
  pinMode(TMP, INPUT);
  pinMode(10, INPUT);
  pinMode(trigPin,OUTPUT);
  pinMode(echoPin,INPUT);
  
  //pinMode(pixels, OUTPUT);
  pinMode(motor, OUTPUT);
  pinMode(peizo, OUTPUT);
  pinMode(LedRed, OUTPUT);
  pinMode(LedGreen, OUTPUT);
  lcd.clear();
  lcd.print("Temp= ");
  lcd.setCursor(7,0);
  lcd.print("|");
  lcd.setCursor(8,0);
  lcd.print("MST= ");
  lcd.setCursor(0,1);
  lcd.print("Motor= ");
}
void scarecrow(){
  digitalWrite(trigPin,LOW);
  delayMicroseconds(2);
  
  digitalWrite(trigPin,HIGH);
  delayMicroseconds(10);
  
  digitalWrite(trigPin,LOW);
    
  float duration = pulseIn(echoPin, HIGH);
  float distance = (duration * 0.017);
  Serial.print("distance: ");
  Serial.print(distance);
  Serial.print("cm");
  Serial.println("");
  delay(100);
  
  if (distance > 32 && distance <322){
    digitalWrite(9, HIGH);
    tone(peizo,294);

  }
  else {
    noTone(peizo);
  }
}
void Neoboard(){
  int value1 = analogRead(SMP);
  float Moisture = value1 * 1/8.75 ;
  if (Moisture < 10){
    for (int i = 0; i < NUMPIXELS; i++) {
      pixels.setPixelColor(i, 255, 0, 0); }
    for (int j = 255; j > 0; j=j-2){
      pixels.setBrightness(j);
      pixels.show();
      //delay(0.1);
    }
  }
  else if (Moisture > 10 && Moisture < 20){
    for (int i = 0; i < NUMPIXELS; i++) {
      pixels.setPixelColor(i, 255, 125, 5); }
    for (int j = 255; j > 0; j=j-2){
      pixels.setBrightness(j);
      pixels.show();
      //delay(0.1);
    }
  }
  else if (Moisture > 20 && Moisture < 30){
    for (int i = 0; i < NUMPIXELS; i++) {
      pixels.setPixelColor(i, 255, 255, 0); }
    for (int j = 255; j > 0; j=j-2){
      pixels.setBrightness(j);
      pixels.show();
      //delay(0.1);
    }
  }
  else if (Moisture > 30 && Moisture < 50){
    for (int i = 0; i < NUMPIXELS; i++) {
      pixels.setPixelColor(i, 0, 255, 0); }
    for (int j = 255; j > 0; j=j-2){
      pixels.setBrightness(j);
      pixels.show();
      //delay(0.1);
    }
  }
  else if (Moisture > 50 && Moisture < 60){
    for (int i = 0; i < NUMPIXELS; i++) {
      pixels.setPixelColor(i, 130, 200, 250); }
    for (int j = 255; j > 0; j=j-2){
      pixels.setBrightness(j);
      pixels.show();
      //delay(0.1);
    }
  }
  else if (Moisture > 60 && Moisture < 75){
    for (int i = 0; i < NUMPIXELS; i++) {
      pixels.setPixelColor(i, 31, 70, 220); }
    for (int j = 255; j > 0; j=j-2){
      pixels.setBrightness(j);
      pixels.show();
      //delay(0.1);
    }
  }
  else if (Moisture > 75){
    for (int i = 0; i < NUMPIXELS; i++) {
      pixels.setPixelColor(i, 20, 30, 250); }
    for (int j = 255; j > 0; j=j-2){
      pixels.setBrightness(j);
      pixels.show();
      //delay(0.1);
    }
  }
}

void loop() {

  int value = analogRead(TMP);
  int Temperature = value * 3/10;
  int value1 = analogRead(SMP);
  float Moisture = value1 * 1/8.75 ;
  lcd.setCursor(5,0);
  lcd.print(Temperature);
  lcd.setCursor(12,0);
  lcd.print(Moisture);
  lcd.setCursor(6,1);
  //pixels.clear();
  pixels.setBrightness(100);
  
  if (Moisture < 25){
    if(Temperature > 45){
    digitalWrite(motor, HIGH);
    digitalWrite(LedRed, HIGH);
    digitalWrite(LedGreen, LOW);
    lcd.print("ON");
      delay(100);
    }
    else{
    digitalWrite(LedRed, HIGH);
    digitalWrite(LedGreen, LOW);
    digitalWrite(motor, HIGH);
    lcd.print("ON ");
      delay(100);
    } 
  }
  else if(Moisture > 25 && Moisture < 75){
    if(Temperature > 45){
    digitalWrite(LedRed, HIGH);
    digitalWrite(LedGreen, LOW);
    digitalWrite(motor, HIGH);
    lcd.print("ON ");
    delay(100);
    }
    else{
    digitalWrite(motor, LOW);
    digitalWrite(LedRed, LOW);
    digitalWrite(LedGreen, HIGH);
    lcd.print("OFF");
    delay(100);
    }
  } 
  else if (Moisture > 75){
    digitalWrite(motor, LOW);
    digitalWrite(LedRed, LOW);
    digitalWrite(LedGreen, HIGH);
    lcd.print("OFF");
    delay(100);
  }
  else {
  exit(0);}
  Neoboard();
  scarecrow();
  delay(100);
}
