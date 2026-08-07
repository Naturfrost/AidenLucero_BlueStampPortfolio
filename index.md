  # ESP32 Weather Station
The ESP32 Weather Station is an open-source weather monitoring system which uses wireless/environmental sensors to collect and display data on weather as well as air quality. This system is built around the ESP32 microcontroller and can be connected using WiFI. I plan on using this project to expand my engineering knowledge while also contributing to an inventory system project for my local cafe.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Aiden L | Oxford Academy | Mechanical/Civil Engineering | Rising Junior

<img width="2880" height="2160" alt="image" src="https://github.com/user-attachments/assets/60ed09c8-bbd5-40c3-be8b-1435721d6310" />

  
# Final Milestone

Complete the development of the weather station, logging weather data over time and adding alerts
https://youtu.be/ooYnj3vi2sI?si=z_g07CqPRGZ3UY5b

Summary:
- In this final version of the project, I went all out on displaying the updated weather system
- Added alerts to keep track of temperature being too high or low along with light status being displayed
- The ESP32 is also connected to an OLED screen which keeps up with tracking the data of the web server
- I plan to incorporate my experiences at BSE into the real world through my projects and future goals.

# Second Milestone

Make the weather station connected to WiFi, displaying live weather data inside a web server hoster by ESP32
https://youtu.be/KzbRhaN9ozo?si=OARWozjAMMT7sy9H

Summary:
- Used LDR and Humidity sensor to detect light / weather activity
- A few problems occurred with the sensors not picking up enough data
- Reworked code and schematics helped fix the data issue
- The web server (once activated) tracks light level, temperature, and humidity % around the ESP32 through the sensors
- Planning to further develop the web environment by adding alerts

# First Milestone

Successfully set up the ESP32 development environment while getting the hardware and sensors to work
https://youtu.be/NzX8GpwYkMw?si=XQsiVzK3EKYiUTat

Summary:
- Got ESP32 web server working on a public ip where you can control the OLED lights
- Turns on/off lights on demand using clickable buttons
- Challenges with connecting the WiFi but solved through hotspot allowing for closer range
- Planning to continue development of the web server using weather data

# Schematics 
<img width="1082" height="906" alt="image" src="https://github.com/user-attachments/assets/a4a24edd-1a69-4b45-baaa-a3d29c2e78c5" />

# Code

// ESP32 Weather Station with OLED
// Install libraries:
// Adafruit GFX Library
// Adafruit SSD1306

```c++
#include <Arduino.h>
#include <WiFi.h>
#include <WebServer.h>
#include <DHT.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

const char* ssid = "Lucero-1";
const char* password = "Roblox2011!!!";

#define LDR_PIN 34
#define DHT_PIN 4
#define DHT_TYPE DHT11

#define COLD_THRESHOLD 15.0
#define HOT_THRESHOLD 30.0

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET -1

Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

DHT dht(DHT_PIN, DHT_TYPE);
WebServer server(80);

int gLight = 0;
float gTemp = NAN;
float gHum = NAN;

unsigned long lastRead = 0;

void readSensors() {
  gLight = analogRead(LDR_PIN);

  float t = dht.readTemperature();
  float h = dht.readHumidity();

  if (!isnan(t)) gTemp = t;
  if (!isnan(h)) gHum = h;
}

void updateOLED() {

  display.clearDisplay();
  display.setTextColor(SSD1306_WHITE);
  display.setTextSize(1);
  display.setCursor(0,0);

  display.println("ESP32 WEATHER");
  display.println();

  display.print("Temp : ");
  if (isnan(gTemp))
    display.println("--");
  else {
    display.print(gTemp,1);
    display.println(" C");
  }

  display.print("Hum  : ");
  if (isnan(gHum))
    display.println("--");
  else {
    display.print(gHum,0);
    display.println("%");
  }

  display.print("Light: ");
  display.println(gLight);

  display.print("Status: ");

  if (gLight < 1000)
    display.println("Dark");

  else if (gLight >= 3000)
    display.println("Bright");

  else
    display.println("Normal");

  display.println();

  if (!isnan(gTemp)) {

    if (gTemp <= COLD_THRESHOLD)
      display.println("ALERT: TOO COLD");

    else if (gTemp >= HOT_THRESHOLD)
      display.println("ALERT: TOO HOT");

    else
      display.println("Weather OK");
  }

  display.display();
}

void handleRoot() {

  String alert = "";
  String bg = "#f4f4f4";
  String lightStatus = "";

  if (gLight < 1000)
    lightStatus = "Dark";

  else if (gLight >= 3000)
    lightStatus = "Bright";

  else
    lightStatus = "Normal";

  if (!isnan(gTemp)) {

    if (gTemp <= COLD_THRESHOLD) {
      bg = "#cce5ff";
      alert = "<h2 style='color:blue'>ALERT: Weather is too cold!";
    }

    else if (gTemp >= HOT_THRESHOLD) {
      bg = "#ffcccc";
      alert = "<h2 style='color:red'>ALERT: Weather is too hot!";
    }
  }

  String html =
    "<!DOCTYPE html>"
    "<html>"
    "<head>"
    "<meta http-equiv='refresh' content='2'>"
    "<style>"
    "body{font-family:Arial;text-align:center;background:" + bg + ";}"
    "</style>"
    "</head>"
    "<body>";

  html += "<h1>ESP32 Weather Station</h1>";
  html += alert;

  html += "<p><b>Temperature:</b> ";
  html += (isnan(gTemp) ? String("--") : String(gTemp,1));
  html += " C</p>";

  html += "<p><b>Humidity:</b> ";
  html += (isnan(gHum) ? String("--") : String(gHum,0));
  html += " %</p>";

  html += "<p><b>Light:</b> ";
  html += String(gLight);
  html += "</p>";

  html += "<p><b>Light Status:</b> ";
  html += lightStatus;
  html += "</p>";

  html += "</body></html>";

  server.send(200, "text/html", html);
}

void handleJson() {

  String j =
    "{\"light\":" + String(gLight) +
    ",\"temp\":" + (isnan(gTemp) ? "null" : String(gTemp,1)) +
    ",\"hum\":" + (isnan(gHum) ? "null" : String(gHum,0)) +
    "}";

  server.send(200, "application/json", j);
}

void setup() {

  Serial.begin(115200);

  dht.begin();

  Wire.begin(21,22);

  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);
  display.clearDisplay();
  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);
  display.setCursor(0,0);
  display.println("Starting...");
  display.display();

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println();
  Serial.print("IP Address: ");
  Serial.println(WiFi.localIP());

  server.on("/", handleRoot);
  server.on("/json", handleJson);

  server.begin();
}

void loop() {

  server.handleClient();

  if (millis() - lastRead > 2000) {

    lastRead = millis();

    readSensors();

    updateOLED();

    Serial.printf("Light:%d Temp:%.1fC Hum:%.0f%%\n",
                  gLight, gTemp, gHum);

    if (gLight < 1000)
      Serial.println("Light Status: DARK");

    else if (gLight >= 3000)
      Serial.println("Light Status: BRIGHT");

    else
      Serial.println("Light Status: NORMAL");
  }
}
```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Female-to-male dupoint wires | Connects the ESP 32 power source to the OLED display screen | $6.99 | <a href="https://www.amazon.com/Solderless-Multicolored-Electronic-Breadboard-Protoboard/dp/B09FPDNHLL/ref=sxin_25_pa_sp_search_thematic_sspa?content-id=amzn1.sym.83f5ab61-f5a2-46fe-8ca0-d4cbfcb9bc51%3Aamzn1.sym.83f5ab61-f5a2-46fe-8ca0-d4cbfcb9bc51&cv_ct_cx=dupont%2Bcable&keywords=dupont%2Bcable&pd_rd_i=B09FPDNHLL&pd_rd_r=1a108cd0-5547-4bcd-81c5-486c97bcad7b&pd_rd_w=6wH4b&pd_rd_wg=adAjS&pf_rd_p=83f5ab61-f5a2-46fe-8ca0-d4cbfcb9bc51&pf_rd_r=QJNEA7JECV19NEM044JE&qid=1784825552&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sr=1-1-e169343e-09af-4d41-85b1-8335fe8f32d0-spons&aref=5iHs3YL1Gs&sp_csd=d2lkZ2V0TmFtZT1zcF9zZWFyY2hfdGhlbWF0aWM&th=1"> Link </a> |
| 830 tie-points breadboard | Used to mount the ESP 32 and duopoint wires | $5.99 | <a href="https://www.amazon.com/EL-CP-003-Breadboard-Solderless-Distribution-Connecting/dp/B01EV6LJ7G/ref=sr_1_4?crid=2787T3XKYJJNM&dib=eyJ2IjoiMSJ9.eV9nARvK2S1_-r45I8kSD3DlFGq0x8DTpVOHrDGu9QwUWkYfJgG3IuujgZls-ZJqQ6SPJUT3GEnXDtVBde6MXHIR6iN-6VAceyyO-Dl-njt9HvOb324l9-3viGzK9zfUldWy4T_Ql8K5nhgBDRqSwLiaiXd5h_qx9HsI3XwjaWNgs_s3TBUUC0FhURF0rP8T2AoVVye7K8zuxADATBpO84e9Ym_Tf7Ss41xN67PD-UXlCOVviUiUNr8wktVTqCrU68ThrQHOmmGRBQkSgj6c8VhhAZ--DHQAOMkkHTblvCY.eWfVWstjiGSPlCknRKMmrUtx7Xy--MWwp2ZP8eUggog&dib_tag=se&keywords=830+tie+points+breadboard&qid=1784753324&s=industrial&sprefix=830+tie+points+breadboar%2Cindustrial%2C171&sr=1-4"> Link </a> |
| 2.4 GHz ESP 32 Board | Microcontroller connected to the computer using a usb cable to control the display on the OLED screen | $8.99 | <a href="https://www.amazon.com/DIYables-ESP-WROOM-32-Development-Microcontroller-Compatible/dp/B0DRBKM49W/ref=asc_df_B0DRBKM49W?tag=bingshoppinga-20&linkCode=df0&hvadid=80745595095959&hvnetw=o&hvqmt=e&hvbmt=be&hvdev=c&hvlocint=&hvlocphy=79126&hvtargid=pla-4584345080865252&psc=1&hvocijid=14526427888366045799-B0DRBKM49W-&hvexpln=0"> Link </a> |
| 0.96 Inch OLED Display Module | High resolution screen which gathers the input from the source code on the computer to the ESP32 microcontroller | $5.99 | <a href="https://www.amazon.com/Dorhea-Display-3-3V-5V-Arduino-Raspberry/dp/B07FK8GB8T/ref=sr_1_4?crid=3ODG0EFJIP6R0&dib=eyJ2IjoiMSJ9.oAZKdk9yLjCgM4o7DR9IOmCC6vjMf_qGiBCsFKKTDXd1UGiFMy_znHd8G_siUbjY6g_-m_bbDEV-6V16zumfqxPBAyMHq7xhZ0MELBBqGfqI_K4GBHieLP0ZsYRHOcvRxmmQt3uVJItjApq8-QjmJul2n2XAeg_Rj-tQomQNxpc5zM_o3l4AvwmwGWq2H2x7j3cpLHA35kRUkDg9TDfQs_S7hXBQPzWylhssNhjEjizMOoxiFlmNjvW1wkRxYO8C1m33iCh2jo1qwSjRCUTbc5sv5ONfgh9HkL7Z1ts4peA.Zr4frmXt4cVqZDoukxB_P3sHYo7k2XD8qTYqaKtlaq8&dib_tag=se&keywords=0.96%2Binch%2Boled%2Bdisplay%2Bmodule&qid=1784753392&s=industrial&sprefix=0.96%2Binch%2Boled%2Bdisplay%2Bmodul%2Cindustrial%2C169&sr=1-4&th=1"> Link </a> |
