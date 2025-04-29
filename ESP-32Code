#include <LiquidCrystal_I2C.h>
#define BLYNK_PRINT Serial
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>
#include <DHT.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);

char auth[] = "4Tw2uOkxaplY6-KKByYGQlDOGHvk73_f"; 
char ssid[] = "Dialog 4G 025";  
char pass[] = "jangu1234";  

#define DHTPIN 15 // Choose GPIO15 or your choice
#define LDRPIN 4 // Digital pin for LDR logic
#define RAINPIN 34 // Analog-capable GPIO for rain sensor

DHT dht(DHTPIN, DHT11);
BlynkTimer timer;

void weather() {
  float h = dht.readHumidity();
  float t = dht.readTemperature();
  int r = analogRead(RAINPIN);
  bool l = digitalRead(LDRPIN);

  r = map(r, 0, 4095, 100, 0); // ESP32 ADC is 12-bit

  if (isnan(h) || isnan(t)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }

  Blynk.virtualWrite(V0, t);
  Blynk.virtualWrite(V1, h);
  Blynk.virtualWrite(V2, r);

  WidgetLED led1(V3);
  if (l == 0) {
    led1.on();
    lcd.setCursor(9, 1);
    lcd.print("L :High ");
  } else {
    led1.off();
    lcd.setCursor(9, 1);
    lcd.print("L :Low ");
  }

  lcd.setCursor(0, 0);
  lcd.print("T :");
  lcd.print(t);

  lcd.setCursor(0, 1);
  lcd.print("H :");
  lcd.print(h);

  lcd.setCursor(9, 0);
  lcd.print("R :");
  lcd.print(r);
  lcd.print(" ");
}

void setup() {
  Serial.begin(9600);
  lcd.init();
  lcd.backlight();
  Blynk.begin(auth, ssid, pass);
  dht.begin();
  pinMode(LDRPIN, INPUT);
  timer.setInterval(1000L, weather);
}

void loop() {
  Blynk.run();
  timer.run();
}
