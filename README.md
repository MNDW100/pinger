# pinger

```cpp
const uint8_t LED_PIN = 9;

void setup() {
  pinMode(LED_PIN, OUTPUT);
  analogWrite(LED_PIN, 0);
  Serial.begin(9600);
}

void loop() {
  int brightness = -1;

  while (Serial.available() > 0) {
    brightness = Serial.read();
  }

  if (brightness >= 0) {
    analogWrite(LED_PIN, brightness);
  }
}
```
