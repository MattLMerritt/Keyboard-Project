//Copy and Paste Machine code.

//######COPY Debouncing###
// constants won't change. They're used here to set pin numbers:
const int COPY_PIN = 6;        // the number of the pushbutton pin
const int PASTE_PIN = 7;
const int DEBOUNCE_DELAY = 50;   // the debounce time; increase if the output flickers

// Variables will change:
int lastSteadyState = LOW;       // the previous steady state from the input pin
int lastFlickerableState = LOW;  // the previous flickerable state from the input pin
int currentState;                // the current reading from the input pin
unsigned long lastDebounceTime = 0;  // the last time the output pin was toggled // for copy

//######PASTE Debouncing####

// Variables will change Paste:
int lastSteadyStatePaste = LOW;       // the previous steady state from the input pin
int lastFlickerableStatePaste = LOW;  // the previous flickerable state from the input pin
int currentStatePaste;
unsigned long lastDebounceTimePaste = 0;  // the last time the output pin was toggled

//OS variables
int currentStateOS;
int OSPin = 8;


//KeyPresses
//#define KEY_LEFT_CTRL_MAC 0xe3
#define KEY_LEFT_CTRL 0x01

uint8_t buf[8] = {0}; // keyboard report buffer


void setup() {
  // initialize serial communication at 9600 bits per second:
  Serial.begin(9600);
  // initialize the pushbutton pin as an pull-up input
  // the pull-up input pin will be HIGH when the switch is open and LOW when the switch is closed.
  pinMode(COPY_PIN, INPUT_PULLUP);
  pinMode(PASTE_PIN, INPUT_PULLUP);

  //Switch for OS
  pinMode(OSPin, INPUT);
}

void loop() {

  // read the state of the switch/button:
  currentState = digitalRead(COPY_PIN);//copy button
  currentStatePaste = digitalRead(PASTE_PIN);//paste button

  //read the state of OS switch
  currentStateOS = digitalRead(OSPin);//OS pin


  // check to see if you just pressed the button
  // (i.e. the input went from LOW to HIGH), and you've waited long enough
  // since the last press to ignore any noise:


  //COPY BUTTON



 
  // If the switch/button changed, due to noise or pressing:
  if (currentState != lastFlickerableState) {
    // reset the debouncing timer
    lastDebounceTime = millis();
    // save the the last flickerable state
    lastFlickerableState = currentState;
  }

  if ((millis() - lastDebounceTime) > DEBOUNCE_DELAY) {
    // whatever the reading is at, it's been there for longer than the debounce
    // delay, so take it as the actual current state:

    // if the button state has changed:
    if (lastSteadyState == HIGH && currentState == LOW) {
      if (currentStateOS == HIGH) { //mac mode
        //Serial.println("The Mac Copy button is pressed");
        buf[0] = 131;   // Ctrl
        buf[2] = 'c';    // letter c
        Serial.write(buf, 8);  // Send keypress
        releaseKey();
        //buf[2] = 124;
      } else { //windows defult = LOW : currentStateOS == HIGH = Mac
        //Serial.println("The Windows Copy button is pressed");
        buf[0] = KEY_LEFT_CTRL;   // Ctrl
        buf[2] = 6;    // letter c
        Serial.write(buf, 8);  // Send keypress
        releaseKey();
      }

    } else if (lastSteadyState == LOW && currentState == HIGH) {
      //Serial.println("The Copy button is released");
    }

    // save the the last steady state
    lastSteadyState = currentState;
  }

  //PASTE BUTTON:


  // If the switch/button changed, due to noise or pressing:
  if (currentStatePaste != lastFlickerableStatePaste) {
    // reset the debouncing timer
    lastDebounceTimePaste = millis();
    // save the the last flickerable state
    lastFlickerableStatePaste = currentStatePaste;
  }

  if ((millis() - lastDebounceTimePaste) > DEBOUNCE_DELAY) {
    // whatever the reading is at, it's been there for longer than the debounce
    // delay, so take it as the actual current state:

    // if the button state has changed:
    if (lastSteadyStatePaste == HIGH && currentStatePaste == LOW) {
     
      if (currentStateOS == HIGH) {
        //Serial.println("The MAC Paste button is pressed");
        buf[0] = 131;
        buf[2] = 25;//command key
        Serial.write(buf, 8);  // Send keypress
        releaseKey();

      } else {
        //Serial.println("The Windows Paste button is pressed");
        buf[0] = KEY_LEFT_CTRL;   // Ctrl
        buf[2] = 25;    // Letter v
        // buf[2] = 125;    // Paste key: Less portable
        Serial.write(buf, 8); // Send keypress
        releaseKey();
      }
    }
    else if (lastSteadyStatePaste == LOW && currentStatePaste == HIGH)
      //Serial.println("The Paste button is released");

    // save the the last steady state
    lastSteadyStatePaste = currentStatePaste;
  }


}

void releaseKey() {
  buf[0] = 0;
  buf[2] = 0;
  Serial.write(buf, 8); //release key
  delay(500);
}
