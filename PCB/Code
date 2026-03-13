#include <MAX6675.h>

const int heater1Pin = 5;
const int heater2Pin = 6;

const int step1Pin = 2;
const int dir1Pin  = 3;
const int en1Pin   = 4;

const int step2Pin = 7;
const int dir2Pin  = 8;
const int en2Pin   = 9;

const int thermoSO  = 12;
const int thermoSCK = 13;
const int thermoCS1 = 10;
const int thermoCS2 = 11;

MAX6675 thermocouple1(thermoSCK, thermoCS1, thermoSO);
MAX6675 thermocouple2(thermoSCK, thermoCS2, thermoSO);

const int greenLED = A0;
const int redLED   = A1;


float targetTemp = 210.0;
const float tolerance = 2.0;

float Kp = 8.0;
float Ki = 0.3;
float Kd = 50.0;

float integral1 = 0;
float lastError1 = 0;

float integral2 = 0;
float lastError2 = 0;

unsigned long lastStepTime = 0;
const int stepDelay = 4; 

bool systemEnabled = true;

unsigned long lastTempRiseTime = 0;
const unsigned long runawayTimeout = 15000; 
float lastTempCheck = 0;


void setup() {
  Serial.begin(115200);

  pinMode(heater1Pin, OUTPUT);
  pinMode(heater2Pin, OUTPUT);

  pinMode(step1Pin, OUTPUT);
  pinMode(dir1Pin, OUTPUT);
  pinMode(en1Pin, OUTPUT);

  pinMode(step2Pin, OUTPUT);
  pinMode(dir2Pin, OUTPUT);
  pinMode(en2Pin, OUTPUT);

  pinMode(greenLED, OUTPUT);
  pinMode(redLED, OUTPUT);

  digitalWrite(en1Pin, LOW);
  digitalWrite(en2Pin, LOW);

  digitalWrite(dir1Pin, HIGH);
  digitalWrite(dir2Pin, HIGH);

  delay(500);
}


void loop() {

  if (!systemEnabled) {
    emergencyStop();
    return;
  }

  float temp1 = thermocouple1.getCelsius();
  float temp2 = thermocouple2.getCelsius();

  if (isnan(temp1) || isnan(temp2) || temp1 < 0 || temp2 < 0) {
    errorFlash();
    systemEnabled = false;
    return;
  }

  int power1 = computePID(temp1, integral1, lastError1);
  int power2 = computePID(temp2, integral2, lastError2);

  analogWrite(heater1Pin, power1);
  analogWrite(heater2Pin, power2);

  if (millis() - lastTempRiseTime > runawayTimeout && temp1 < targetTemp - 15) {
    Serial.println("Thermal Runaway Detected!");
    systemEnabled = false;
  }

  if (temp1 > lastTempCheck + 2) {
    lastTempRiseTime = millis();
    lastTempCheck = temp1;
  }

  if (temp1 < targetTemp - tolerance || temp2 < targetTemp - tolerance) {
    digitalWrite(greenLED, HIGH);  
    digitalWrite(redLED, LOW);
  } else {
    digitalWrite(greenLED, LOW);
    digitalWrite(redLED, HIGH);    
  }

  runMotors();

  Serial.print("T1: ");
  Serial.print(temp1);
  Serial.print("  T2: ");
  Serial.print(temp2);
  Serial.print("  P1: ");
  Serial.print(power1);
  Serial.print("  P2: ");
  Serial.println(power2);

  checkSerialCommands();
}


int computePID(float currentTemp, float &integral, float &lastError) {
  float error = targetTemp - currentTemp;

  integral += error;
  float derivative = error - lastError;
  lastError = error;

  float output = (Kp * error) + (Ki * integral) + (Kd * derivative);

  output = constrain(output, 0, 255);
  return (int)output;
}


void runMotors() {
  unsigned long now = millis();
  if (now - lastStepTime >= stepDelay) {
    lastStepTime = now;

    digitalWrite(step1Pin, HIGH);
    digitalWrite(step2Pin, HIGH);
    delayMicroseconds(600);
    digitalWrite(step1Pin, LOW);
    digitalWrite(step2Pin, LOW);
  }
}


void errorFlash() {
  analogWrite(heater1Pin, 0);
  analogWrite(heater2Pin, 0);

  digitalWrite(en1Pin, HIGH);
  digitalWrite(en2Pin, HIGH);

  while (true) {
    digitalWrite(greenLED, HIGH);
    digitalWrite(redLED, HIGH);
    delay(300);
    digitalWrite(greenLED, LOW);
    digitalWrite(redLED, LOW);
    delay(300);
  }
}

void emergencyStop() {
  analogWrite(heater1Pin, 0);
  analogWrite(heater2Pin, 0);
  digitalWrite(en1Pin, HIGH);
  digitalWrite(en2Pin, HIGH);
  errorFlash();
}

void checkSerialCommands() {
  if (Serial.available()) {
    String cmd = Serial.readStringUntil('\n');
    cmd.trim();

    if (cmd.startsWith("SET ")) {
      targetTemp = cmd.substring(4).toFloat();
      Serial.print("New Target Temp: ");
      Serial.println(targetTemp);
    }

    if (cmd == "STOP") {
      systemEnabled = false;
    }
  }
}
