  # ESP32 Weather Station
The ESP32 Weather Station is an open-source weather monitoring system which uses wireless/environmental sensors to collect and display data on weather as well as air quality.This system is built around the ESP32 microcontroller and can be connected using WiFI. 

You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions:
```HTML 
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->
```

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Aiden L | Oxford Academy | Mechanical/Civil Engineering | Rising Junior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

Complete the development of the weather station, logging weather data over time and adding alerts

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



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
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
/*********
  Rui Santos
  Complete project details at https://randomnerdtutorials.com

  Jake Pong
  7/21/2026
  ESP32 Dev Board 
*********/

// esp32 Wifi LED demo

// Load Wi-Fi library
#include <WiFi.h>

// Replace with your network credentials
const char* ssid = "SSID";
const char* password = "PASSWORD";
      

/*// Replace with your network credentials
const char* ssid = "Enter SSID";
const char* password = "Enter password";
*/

// Set web server port number to 80
WiFiServer server(80);

// Variable to store the HTTP request
String header;

// Auxiliar variables to store the current output state
String output26State = "off";
String output27State = "off";

// Assign output variables to GPIO pins
const int output26 = 26;
const int output27 = 27;

// Current time
unsigned long currentTime = millis();
// Previous time
unsigned long previousTime = 0; 
// Define timeout time in milliseconds (example: 2000ms = 2s)
const long timeoutTime = 2000;

void setup() {
  Serial.begin(115200);
  // Initialize the output variables as outputs
  pinMode(output26, OUTPUT);
  pinMode(output27, OUTPUT);
  // Set outputs to LOW
  digitalWrite(output26, LOW);
  digitalWrite(output27, LOW);

  // wifi list
  Serial.println("Scanning WiFi...");

  int networks = WiFi.scanNetworks();

  for (int i = 0; i < networks; i++) {
    Serial.print(i);
    Serial.print(": ");
    Serial.println(WiFi.SSID(i));
  }

  // Connect to Wi-Fi network with SSID and password
  Serial.print("Connecting to ");
  Serial.println(ssid);

  // set wifi mode
  WiFi.mode(WIFI_STA);
  WiFi.disconnect();
  delay(100);

  WiFi.begin(ssid, password);

  // wifi error decode
  while (WiFi.status() != WL_CONNECTED) {

    delay(500);

    Serial.print("WiFi status: ");

    switch(WiFi.status()) {

      case WL_NO_SHIELD:
        Serial.println("No WiFi shield");
        break;

      case WL_IDLE_STATUS:
        Serial.println("Idle");
        break;

      case WL_NO_SSID_AVAIL:
        Serial.println("SSID not found");
        break;

      case WL_CONNECT_FAILED:
        Serial.println("Connection failed");
        break;

      case WL_CONNECTION_LOST:
        Serial.println("Connection lost");
        break;

      case WL_DISCONNECTED:
        Serial.println("Disconnected");
        break;

      default:
        Serial.println("Unknown");
    }
  }

  // Print local IP address and start web server
  Serial.println("");
  Serial.println("WiFi connected.");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
  server.begin();
}

void loop(){
  WiFiClient client = server.available();   // Listen for incoming clients

  if (client) {                             // If a new client connects,
    currentTime = millis();
    previousTime = currentTime;
    Serial.println("New Client.");          // print a message out in the serial port
    String currentLine = "";                // make a String to hold incoming data from the client
    while (client.connected() && currentTime - previousTime <= timeoutTime) {  // loop while the client's connected
      currentTime = millis();
      if (client.available()) {             // if there's bytes to read from the client,
        char c = client.read();             // read a byte, then
        Serial.write(c);                    // print it out the serial monitor
        header += c;
        if (c == '\n') {                    // if the byte is a newline character
          // if the current line is blank, you got two newline characters in a row.
          // that's the end of the client HTTP request, so send a response:
          if (currentLine.length() == 0) {
            // HTTP headers always start with a response code (e.g. HTTP/1.1 200 OK)
            // and a content-type so the client knows what's coming, then a blank line:
            client.println("HTTP/1.1 200 OK");
            client.println("Content-type:text/html");
            client.println("Connection: close");
            client.println();
            
            // turns the GPIOs on and off
            if (header.indexOf("GET /26/on") >= 0) {
              Serial.println("GPIO 26 on");
              output26State = "on";
              digitalWrite(output26, HIGH);
            } else if (header.indexOf("GET /26/off") >= 0) {
              Serial.println("GPIO 26 off");
              output26State = "off";
              digitalWrite(output26, LOW);
            } else if (header.indexOf("GET /27/on") >= 0) {
              Serial.println("GPIO 27 on");
              output27State = "on";
              digitalWrite(output27, HIGH);
            } else if (header.indexOf("GET /27/off") >= 0) {
              Serial.println("GPIO 27 off");
              output27State = "off";
              digitalWrite(output27, LOW);
            }
            
            // Display the HTML web page
            client.println("<!DOCTYPE html><html>");
            client.println("<head><meta name=\"viewport\" content=\"width=device-width, initial-scale=1\">");
            client.println("<link rel=\"icon\" href=\"data:,\">");
            // CSS to style the on/off buttons 
            // Feel free to change the background-color and font-size attributes to fit your preferences
            client.println("<style>html { font-family: Helvetica; display: inline-block; margin: 0px auto; text-align: center;}");
            client.println(".button { background-color: #4CAF50; border: none; color: white; padding: 16px 40px;");
            client.println("text-decoration: none; font-size: 30px; margin: 2px; cursor: pointer;}");
            client.println(".button2 {background-color: #555555;}</style></head>");
            
            // Web Page Heading
            client.println("<body><h1>ESP32 Web Server</h1>");
            
            // Display current state, and ON/OFF buttons for GPIO 26  
            client.println("<p>GPIO 26 - State " + output26State + "</p>");
            // If the output26State is off, it displays the ON button       
            if (output26State=="off") {
              client.println("<p><a href=\"/26/on\"><button class=\"button\">ON</button></a></p>");
            } else {
              client.println("<p><a href=\"/26/off\"><button class=\"button button2\">OFF</button></a></p>");
            } 
               
            // Display current state, and ON/OFF buttons for GPIO 27  
            client.println("<p>GPIO 27 - State " + output27State + "</p>");
            // If the output27State is off, it displays the ON button       
            if (output27State=="off") {
              client.println("<p><a href=\"/27/on\"><button class=\"button\">ON</button></a></p>");
            } else {
              client.println("<p><a href=\"/27/off\"><button class=\"button button2\">OFF</button></a></p>");
            }
            client.println("</body></html>");
            
            // The HTTP response ends with another blank line
            client.println();
            // Break out of the while loop
            break;
          } else { // if you got a newline, then clear currentLine
            currentLine = "";
          }
        } else if (c != '\r') {  // if you got anything else but a carriage return character,
          currentLine += c;      // add it to the end of the currentLine
        }
      }
    }
    // Clear the header variable
    header = "";
    // Close the connection
    client.stop();
    Serial.println("Client disconnected.");
    Serial.println("");
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

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
