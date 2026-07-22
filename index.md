# ESP32 Weather Station
The ESP32 Weather Station is an open-source weather monitoring system which uses wireless/environmental sensors to collect and display data on weather as well as air quality.This system is built around the ESP32 microcontroller and can be connected using WiFI. 

You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions:
```HTML 
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->
```

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Aiden L | Oxford Academy | Mechanical/Civil Engineering | RIsing Junior

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

<iframe width="560" height="315" src="https://www.youtube.com/embed/y3VAmNlER5Y" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your second milestone, explain what you've worked on since your previous milestone. You can highlight:
- Technical details of what you've accomplished and how they contribute to the final goal
- What has been surprising about the project so far
- Previous challenges you faced that you overcame
- What needs to be completed before your final milestone 

# First Milestone

Successfully set up the ESP32 development environment while getting the hardware and sensors to work

<iframe width="560" height="315" src="https://www.youtube.com/embed/CaCazFBhYKs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your first milestone, describe what your project is and how you plan to build it. You can include:
- An explanation about the different components of your project and how they will all integrate together
- Technical progress you've made so far
- Challenges you're facing and solving in your future milestones
- What your plan is to complete your project

# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  Serial.println("Hello World!");
}

void loop() {
  // put your main code here, to run repeatedly:

}
```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Female-to-male dupoint wires | Connects the ESP 32 power source to the OLED display screen | $6.99 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Solderless-Multicolored-Electronic-Breadboard-Protoboard/dp/B09FPDNHLL/ref=sxin_19_pa_sp_search_thematic_sspa?adgrpid=1342504263307483&content-id=amzn1.sym.292df443-b323-44ae-8b40-9a666975b8b5%3Aamzn1.sym.292df443-b323-44ae-8b40-9a666975b8b5&cv_ct_cx=female+to+male+dupont+wire&gb=2&hvadid=83906731247531&hvbmt=be&hvdev=c&hvexpln=0&hvlocphy=79126&hvnetw=o&hvocijid=13119438861593004571--&hvqmt=e&hvtargid=kwd-83906865102284%3Aloc-190&hydadcr=19137_13351467&keywords=female+to+male+dupont+wire&mcid=1a797ac61d8c33b9a39be95441a1f61e&pd_rd_i=B09FPDNHLL&pd_rd_r=970dd41f-2fbd-44a9-ac1a-fb80bdb19f77&pd_rd_w=RyJF9&pd_rd_wg=0UzTU&pf_rd_p=292df443-b323-44ae-8b40-9a666975b8b5&pf_rd_r=0DNNGHZ503VEYRCWTYV1&qid=1784753009&s=electronics&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sr=1-1-6024b2a3-78e4-4fed-8fed-e1613be3bcce-spons&aref=5iHs3YL1Gs&sp_csd=d2lkZ2V0TmFtZT1zcF9zZWFyY2hfdGhlbWF0aWM&psc=1)"> Link </a> |
| 830 tie-points breadboard | Used to mount the ESP 32 and duopoint wires | $5.99 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/EL-CP-003-Breadboard-Solderless-Distribution-Connecting/dp/B01EV6LJ7G/ref=sr_1_4?crid=2787T3XKYJJNM&dib=eyJ2IjoiMSJ9.eV9nARvK2S1_-r45I8kSD3DlFGq0x8DTpVOHrDGu9QwUWkYfJgG3IuujgZls-ZJqQ6SPJUT3GEnXDtVBde6MXHIR6iN-6VAceyyO-Dl-njt9HvOb324l9-3viGzK9zfUldWy4T_Ql8K5nhgBDRqSwLiaiXd5h_qx9HsI3XwjaWNgs_s3TBUUC0FhURF0rP8T2AoVVye7K8zuxADATBpO84e9Ym_Tf7Ss41xN67PD-UXlCOVviUiUNr8wktVTqCrU68ThrQHOmmGRBQkSgj6c8VhhAZ--DHQAOMkkHTblvCY.eWfVWstjiGSPlCknRKMmrUtx7Xy--MWwp2ZP8eUggog&dib_tag=se&keywords=830+tie+points+breadboard&qid=1784753324&s=industrial&sprefix=830+tie+points+breadboar%2Cindustrial%2C171&sr=1-4)"> Link </a> |
| 2.4 GHz ESP 32 Board | Microcontroller connected to the computer using a usb cable to control the display on the OLED screen | $8.99 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/DIYables-ESP-WROOM-32-Development-Microcontroller-Compatible/dp/B0DRBKM49W/ref=asc_df_B0DRBKM49W?tag=bingshoppinga-20&linkCode=df0&hvadid=80745595095959&hvnetw=o&hvqmt=e&hvbmt=be&hvdev=c&hvlocint=&hvlocphy=79126&hvtargid=pla-4584345080865252&psc=1&hvocijid=14526427888366045799-B0DRBKM49W-&hvexpln=0)"> Link </a> |
| 0.96 Inch OLED Display Module | High resolution screen which gathers the input from the source code on the computer to the ESP32 microcontroller | $5.99 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Dorhea-Display-3-3V-5V-Arduino-Raspberry/dp/B07FK8GB8T/ref=sr_1_4?crid=3ODG0EFJIP6R0&dib=eyJ2IjoiMSJ9.oAZKdk9yLjCgM4o7DR9IOmCC6vjMf_qGiBCsFKKTDXd1UGiFMy_znHd8G_siUbjY6g_-m_bbDEV-6V16zumfqxPBAyMHq7xhZ0MELBBqGfqI_K4GBHieLP0ZsYRHOcvRxmmQt3uVJItjApq8-QjmJul2n2XAeg_Rj-tQomQNxpc5zM_o3l4AvwmwGWq2H2x7j3cpLHA35kRUkDg9TDfQs_S7hXBQPzWylhssNhjEjizMOoxiFlmNjvW1wkRxYO8C1m33iCh2jo1qwSjRCUTbc5sv5ONfgh9HkL7Z1ts4peA.Zr4frmXt4cVqZDoukxB_P3sHYo7k2XD8qTYqaKtlaq8&dib_tag=se&keywords=0.96%2Binch%2Boled%2Bdisplay%2Bmodule&qid=1784753392&s=industrial&sprefix=0.96%2Binch%2Boled%2Bdisplay%2Bmodul%2Cindustrial%2C169&sr=1-4&th=1)"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
