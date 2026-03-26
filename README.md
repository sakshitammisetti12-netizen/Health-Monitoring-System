#include <LiquidCrystal.h>

// LCD pins: RS, E, D4, D5, D6, D7
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

int tempPin = A0;
int pirPin = 7;
int trigPin = 9;
int echoPin = 10;
int buzzer = 8;

int button1 = 6;
int button2 = A1;
int button3 = A2;

long duration;
int distance;

void setup()
{
  lcd.begin(16, 2);

  pinMode(pirPin, INPUT);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(buzzer, OUTPUT);

  pinMode(button1, INPUT_PULLUP);
  pinMode(button2, INPUT_PULLUP);
  pinMode(button3, INPUT_PULLUP);

  lcd.print("Health Monitor");
  delay(2000);
  lcd.clear();
}

void loop()
{
  // ----- Temperature -----
  int val = analogRead(tempPin);
  float voltage = val * 5.0;
  voltage /= 1024.0;
  float temp = (voltage - 0.5) * 100;

  // ----- Ultrasonic -----
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;

  // ----- PIR -----
  int motion = digitalRead(pirPin);

  lcd.setCursor(0, 0);
  lcd.print("Temp:");
  lcd.print(temp);
  lcd.print("C ");

  lcd.setCursor(0, 1);
  lcd.print("Dist:");
  lcd.print(distance);
  lcd.print("cm ");

  // ----- Motion alert -----
  if (motion == HIGH)
  {
    digitalWrite(buzzer, HIGH);
  }
  else
  {
    digitalWrite(buzzer, LOW);
  }

  // ----- Button1 -----
  if (digitalRead(button1) == LOW)
  {
    lcd.clear();
    lcd.print("Pulse Check");
    delay(1000);
  }

  // ----- Button2 -----
  if (digitalRead(button2) == LOW)
  {
    lcd.clear();
    lcd.print("BMI Check");
    delay(1000);
  }

  // ----- Button3 -----
  if (digitalRead(button3) == LOW)
  {
    lcd.clear();
    lcd.print("Alert Mode");
    digitalWrite(buzzer, HIGH);
    delay(1000);
    digitalWrite(buzzer, LOW);
  }

  delay(500);
}
