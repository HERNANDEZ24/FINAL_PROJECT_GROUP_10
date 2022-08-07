# FINAL_PROJECT_GROUP_10
 
 
#include<Wire.h>
#include <IRremote.h>
#include <LiquidCrystal_I2C.h>
LiquidCrystal_I2C lcd1(32, 16, 2);
LiquidCrystal_I2C lcd2(33, 16, 2);

int redLed = 12;
int rLed = 10;
int gLed = 2;
int bLed = 11;
int pirPin = 4;
int pirVal; 

int pbPin = 3;
int pbNew=0;
int pbPast=1;

int relayPin = 7;
int IRpin = 9;
IRrecv IR(IRpin);


int trigPin = 13;
int echoPin = 6;
int distance;
int duration;
int t = 1000;

unsigned long fromStartms;
unsigned long prevpirms; 
unsigned long prevms;
unsigned long pbPinms;
unsigned long pirValms;
unsigned long IRpinms;
unsigned long prevIRpinms;
unsigned long prevpbPinms;
unsigned long prevpirValms;
unsigned long pbPin_interval=100;
unsigned long pirVal_interval=100;
unsigned long IRpin_interval=100;
unsigned long interval=250;

void setup()
{
  pinMode(rLed, OUTPUT);
  pinMode(gLed, OUTPUT);
  pinMode(bLed, OUTPUT);
  //DC MOTOR RELAY
  pinMode(relayPin, OUTPUT);
  //ULTRASONIC DISTANCE SENSOR
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
 
  
  //PIR SENSOR
  pinMode(pirPin, INPUT);
  pinMode(pirVal, OUTPUT);
  
  //PUSHBUTTON
  pinMode(pbPin, INPUT);
  digitalWrite(pbPin,HIGH);
  //IR SENSOR
  pinMode(IRpin,INPUT);
  IR.enableIRIn();
  
  lcd1.begin(16,2);
  lcd1.init();
  lcd1.backlight();
  
  lcd2.begin(16,2);
  lcd2.init();
  lcd2.backlight();
}  
void loop()
{
  fromStartms=millis(); 

  if (fromStartms - prevms >interval)
  {  
     prevms = fromStartms;
     pirVal=digitalRead(pirPin);
    
  if (pirVal==1)
  { 
    digitalWrite(relayPin, HIGH);
    digitalWrite(rLed, LOW);
    digitalWrite(gLed, HIGH);
    digitalWrite(bLed, LOW);
    lcd1.setCursor(4,0);
    lcd1.print("HELLO!!!");
    lcd1.setCursor(2,1);
    lcd1.print("PLEASE ENTER");
    delay(5000);
    lcd1.clear();
    lcd1.setCursor(4,0);
    lcd1.print("THANK YOU!!");
    delay(5000);
    lcd1.clear();
  }

  else if (pirVal==0)
  {
    digitalWrite(relayPin, LOW);
    digitalWrite(rLed, LOW);
    digitalWrite(gLed, LOW);
    digitalWrite(bLed, HIGH);
    delay(t);
    lcd1.clear();
    
  }
   if(fromStartms - prevpirms >=pirVal_interval)
  {
    prevpirms = fromStartms;
    if(pirVal==1)
    {
      digitalWrite(trigPin, LOW); 
      delayMicroseconds(2);
      digitalWrite(trigPin, HIGH);
      delayMicroseconds(10);
      digitalWrite(trigPin, LOW);
      duration = pulseIn(echoPin, HIGH);
      distance= (duration*0.034)/2;   
    }
     else if(pirVal==0)
     {
      lcd2.setCursor(0,0);
      lcd2.print("Distance = ");
      lcd2.setCursor(10,0);
      lcd2.print(distance);
      lcd2.setCursor(14,0);
      lcd2.print("cm");
    }
   
{
      
  Serial.println(pirVal);
  
  if (fromStartms - IRpinms > interval)
 
    prevms = fromStartms;
    IRpin=digitalRead(IRpin);
   
  while(IR.decode())
  
//POWER OFF BUTTON
 if(IR.decodedIRData.decodedRawData==0xFF00BF00)
 {
  Serial.println("TURN OFF");
  digitalWrite(relayPin, LOW);
  digitalWrite(rLed,HIGH);
  digitalWrite(gLed,LOW);
  digitalWrite(bLed,LOW);
  delay(t);
  lcd1.clear();
  lcd2.clear();
 }
 
  IR.resume();  
}
}
}
}
