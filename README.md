# 🔔 Motion Activated Alarm System using Robotics and IoT

This project implements a smart motion detection and alarm system using a **PIR sensor and Arduino**, enhanced with **IoT integration**. It detects human motion and triggers an alarm or LED light — suitable for home security or automation systems.

---

## 📹 Demo Video

[▶️ Watch Working Demo](https://drive.google.com/file/d/10wi6nJRIKcpJRlsJVLjRtLM2H4lbL5fI/view?usp=drive_link)  

---

## 📄 Full Project Report

[📄 View Full Report (DOCX)](https://docs.google.com/document/d/1Z02YrdtFZLhA0uUJQOz6JASi0U0xSoIKklFNi_g0SY4/edit?usp=sharing)  

---

## 🛠️ Components Used

- Arduino Uno
- PIR Motion Sensor (HC-SR501)
- LED (or buzzer)
- Jumper wires & breadboard
- Optional: Wi-Fi/Bluetooth Module for IoT integration

---

## ⚙️ Working Principle

- The PIR sensor detects motion by sensing changes in infrared radiation.
- On detecting motion:
  - An LED (or buzzer) is triggered.
  - Timestamped alerts are printed to the serial monitor.
- Once motion stops for 5 seconds, the alarm deactivates.
- The system includes a 30-second calibration time on startup.

---

## 💻 Code

```cpp
int calibrationTime = 30;
long unsigned int lowIn;
long unsigned int pause = 5000;

boolean lockLow = true;
boolean takeLowTime;

int pirPin = 3;
int ledPin = 13;

void setup(){
  Serial.begin(9600);
  pinMode(pirPin, INPUT);
  pinMode(ledPin, OUTPUT);
  digitalWrite(pirPin, LOW);
  Serial.print("Calibrating sensor ");
  for(int i = 0; i < calibrationTime; i++){
    Serial.print(".");
    delay(1000);
  }
  Serial.println(" done");
  Serial.println("SENSOR ACTIVE");
  delay(50);
}

void loop(){
  if(digitalRead(pirPin) == HIGH){
    digitalWrite(ledPin, HIGH);
    if(lockLow){
      lockLow = false;
      Serial.println("---");
      Serial.print("Motion detected at ");
      Serial.print(millis() / 1000);
      Serial.println(" sec");
      delay(50);
    }
    takeLowTime = true;
  }

  if(digitalRead(pirPin) == LOW){
    digitalWrite(ledPin, LOW);
    if(takeLowTime){
      lowIn = millis();
      takeLowTime = false;
    }
    if(!lockLow && millis() - lowIn > pause){
      lockLow = true;
      Serial.print("Motion ended at ");
      Serial.print((millis() - pause) / 1000);
      Serial.println(" sec");
      delay(50);
    }
  }
}
