# mbayet.github.io
                                               introducing my self 

My name is Mbaye Toure ,and I am going to be a sophmore next year at wisdom Highschool in HTX . This summer I made a ultrasonic security system. My ultasonic security system how does it work and what does it do ? well let me tell you. 

                                                      ultrasonic security system 
My ultrasonic system it detects how close someone is to my sensor when that hapen my light will turn off showing that it is detecting something, and if you are trying to access it you will have to put in the pasword and scan my rfid card.
I am going to Show you a picture of my system then I will tell how everything works, and how I did it .

                                                      picture
![Alt text](WIN_20230716_23_34_59_Pro.jpg)
                                                  ultra sonic sensor
In this project I first connected my ultrasonic sensor to my elego mega 2560 R3 wich is basicly the brain of the whole project I had to connect my vcc wich is the 5 volt I also conected the trig pin the echo,and my ground . The echo pin and the trig pin are digital pins ,but what I did is that I decided to conect my digtal pins through my breadboard using long male to male wires  so I can have more space and would not have trouble,and to make my life easier the breadboard is where you develope an electronic circuit and wiring.
                                                  RFID
After conecting my sensor I proceded to move on and connect my rfid for those who dont know what an rfid is it is a Radio Frequency Identification system where you can scan your card . I also connected My rfid through my breadboard just to make my life easier .
                                                    Keypad
After connecting my rfid i conected my keypad  where I will be able to enter my pascode this time instad of connecting my keypad to my breadboard I decided to connect it through my  elego mega 2560 R3 wich I call  the brain , because it was better for me so I can have more space.
                                                     RGB
When i finished conecting my keypad i connected my rgb light wich is an led. I also used 220 ohm resistor so I can limits and regulate the flow of electrical current in an electronic circuit. one thing you need to knoww is that the rgb light has 4 wires and one of them is very long and,it is called a cathode that is where you will connect your ground for the light to be able to turn on and the other three wires you will conect your 3 resistors.
                                                     CODE
After you finish connecting evrything you need you will need to code your project using arduino I will be puting my code on here for those of you who want to make the same exact project I made .
```C++
  #include <SPI.h>
/* Include the RFID library */
#include <RFID.h>
#include "SR04.h"
#include <Keypad.h>

#define TRIG_PIN 10
#define ECHO_PIN 11
#define SDA_DIO 9
#define RESET_DIO 8
#define BLUE 4
#define GREEN 3
#define RED 2


//KeyPad begin setup
const byte ROWS = 4;  //four rows
const byte COLS = 4;  //four columns
//define the cymbols on the buttons of the keypads
char hexaKeys[ROWS][COLS] = {
  { '1', '2', '3', 'A' },
  { '4', '5', '6', 'B' },
  { '7', '8', '9', 'C' },
  { '*', '0', '#', 'D' }
};
byte rowPins[ROWS] = { 23, 27, 31, 35 };  //connect to the row pinouts of the keypad
byte colPins[COLS] = { 39, 43, 47, 24 };  //connect to the column pinouts of the keypad
Keypad customKeypad = Keypad(makeKeymap(hexaKeys), rowPins, colPins, ROWS, COLS);

//KeyPad end setup

RFID RC522(SDA_DIO, RESET_DIO);
SR04 sr04 = SR04(ECHO_PIN, TRIG_PIN);
long distance;
char customKey = customKeypad.getKey();
char enteredKey = ' ';
/* Define the DIO used for the SDA (SS) and RST (reset) pins. */
/*
PINOUT:
RC522 MODULE    Uno/Nano     MEGA
SDA             D10          D11
SCK             D13          D52
MOSI            D11          D51
MISO            D12          D50
IRQ             N/A          N/A
GND             GND          GND
RST             D9           D8
3.3V            3.3V         3.3V
*/

/* Create an instance of the RFID library */

void setup() {
  Serial.begin(9600);
  /* Enable the SPI interface */
  SPI.begin();
  /* Initialise the RFID reader */
  RC522.init();

  pinMode(RED, OUTPUT);
  pinMode(GREEN, OUTPUT);
  pinMode(BLUE, OUTPUT);
  digitalWrite(RED, LOW);
  digitalWrite(GREEN, LOW);
  digitalWrite(BLUE, LOW);
}

void loop() {
  distance = sr04.Distance();
  // We start with if distance is < 100
  if (distance < 100) {
    digitalWrite(BLUE, LOW);
    // Serial.println("pls scan now");
    if (RC522.isCard()) {
      /* If so then get its serial number */
      String cardNumberScanned = "";
      RC522.readCardSerial();
      Serial.println("Card detected:");
      for (int i = 0; i < 5; i++) {
        // Serial.print(RC522.serNum[i], DEC);
        cardNumberScanned.concat(RC522.serNum[i]);
      }
      Serial.println();
      Serial.print("card Number: ");
      Serial.println(cardNumberScanned);
      if (cardNumberScanned.compareTo("13646392239") == 0) {
        if (enteredKey == '1') {
          digitalWrite(RED, LOW);
          digitalWrite(GREEN, LOW);
          digitalWrite(BLUE, LOW);
          delay(300);
          digitalWrite(RED, LOW);
          digitalWrite(GREEN, HIGH);
        } else {
          Serial.print("incorrect code: ");
          Serial.println(enteredKey);
          digitalWrite(RED, LOW);
          digitalWrite(GREEN, LOW);
          digitalWrite(BLUE, LOW);
          delay(300);
          digitalWrite(RED, HIGH);
          digitalWrite(GREEN, LOW);
        }

      } else {
        digitalWrite(RED, LOW);
        digitalWrite(GREEN, LOW);
        digitalWrite(BLUE, LOW);
        delay(300);
        digitalWrite(RED, HIGH);
        digitalWrite(GREEN, LOW);
        Serial.println("no card incorrect");
      }
      Serial.println();
      Serial.println();
    }
  } else {
    digitalWrite(BLUE, HIGH);
  }
  /* Has distance card been detected? */
  customKey = customKeypad.getKey();
  if (customKey && enteredKey != customKey) {
    Serial.println(customKey);
    enteredKey = customKey;
  }
  if (customKey == 'D') {
    Serial.print(distance);
    Serial.println("cm");
  }
}
```
