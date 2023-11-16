//trig pin(ultrasonic sensor)->12
//echo pin(ultrasonic sensor)->13
//SDA(Oled)->A4
//SCK(Oled)->A5
//Vcc(Oled)->3.3V
//Gnd(both)->GND
//Vcc(ultrasonic sensor)->5v


//CODE
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <NewPing.h>

// Define pins for the ultrasonic sensor
const int trigPin = 12;
const int echoPin = 13;

// Define pins for the OLED display
const int oledReset = 4;

// Define the maximum distance to be measured
const int maxDistance = 200; // in centimeters

// Create an instance of the NewPing class
NewPing sonar(trigPin, echoPin, maxDistance);

// Create an instance of the Adafruit_SSD1306 class
Adafruit_SSD1306 display(oledReset);

void setup() {
  // Initialize the serial port for debugging
  Serial.begin(9600);

  // Initialize the OLED display
  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);
  display.setTextSize(2); // Change the text size to 2 for larger text
  display.setTextColor(WHITE);
  display.clearDisplay();
}

void loop() {
  // Measure the distance in centimeters
  int distance = sonar.ping_cm();

  // Check for errors
  if (distance == 0) {
    Serial.println("No ping received");
    delay(50);
    return;
  }

  // Display the distance on the OLED display
  display.clearDisplay();
  display.setCursor(0, 0);
  display.print("Distance: ");
  display.print(distance);
  display.print(" cm");
  display.display();

  // Print the distance to the serial port
  Serial.println("Distance: ");
  Serial.println(distance);

  delay(100);
}
