#define BLYNK_TEMPLATE_ID           "TMPL4rAtiD1Q"
#define BLYNK_DEVICE_NAME           "Quickstart Device"
#define BLYNK_AUTH_TOKEN            "z5wwf_KdnRyYUITuAu3rd68gDSLh6Aj4"
// Comment this out to disable prints and save space
#define BLYNK_PRINT Serial
#define EspSerial Serial1
#define ESP8266_BAUD 115200
#define IR_SEND_PIN 7

#include <Stepper.h>
#include <ESP8266_Lib.h>
#include <BlynkSimpleShieldEsp8266.h>
#include <IRremote.h>
Stepper myStepper = Stepper(100, 3, 4);

char auth[] = BLYNK_AUTH_TOKEN;
//char ssid[] = "Router";
//char pass[] = "03032000";

char ssid[] = "Bersele";
char pass[] = "harapalb";

//char ssid[] = "205";
//char pass[] = "camera205";

ESP8266 wifi(&EspSerial);

//RECEIVER
const int irReceiverPin = 2;  //the SIG of receiver module attach to pin2
IRrecv irrecv(irReceiverPin); //Creates a variable of type IRrecv
decode_results results;    // define results
//EMITTER
IRsend irsend;

bool turnLeft, turnRight;
String foodStatus="";
BlynkTimer timer;

void setup() {
  // Debug console
  Serial.begin(115200);

  /*WIFI*/
  EspSerial.begin(ESP8266_BAUD);   // Set ESP8266 baud rate
  delay(10);
  Blynk.begin(auth, wifi, ssid, pass);

  /*MOTOR*/
  myStepper.setSpeed(400);

  /*IR SENZORI*/
  irsend.enableIROut(38);  //EMITOR
  irrecv.enableIRIn();    //RECEIVER -> enable ir receiver module

  /*in fiecare secunda se apeleaza myTimerEvent pt a trimite foodStatus la Blynk*/
  timer.setInterval(1000L, myTimerEvent);

  /*2 butoane*/
  pinMode(5, INPUT_PULLUP);
  pinMode(6, INPUT_PULLUP);
}

void myTimerEvent()
{
  Blynk.virtualWrite(V7, foodStatus);
}

BLYNK_WRITE(V0)
{

  Blynk.virtualWrite(V4, 1023);
  Blynk.virtualWrite(V4, 0);

}

BLYNK_WRITE(V5) // check the button state associated to the virtual pin 5
{
  turnLeft = param.asInt(); //read the virtual pin 5 value
}

BLYNK_WRITE(V6)
{
  turnRight = param.asInt();
}

void loop() {

  //emitorul trimite semnal la receiver
  irsend.sendSony(0x68B92, 20);

  if (irrecv.decode(&results)) //if the ir receiver module receiver data
  {
    if (results.value == 0x68B92)
     foodStatus="Cainele NU mai are mancare";
    
    else
       foodStatus="Cainele are destula mancare";
       
   irrecv.resume();    // Receive the next value    
  }

 
  if (turnLeft)
    myStepper.step(100);

  if (turnRight)
    myStepper.step(-100);

  if (digitalRead(5) == LOW)
    myStepper.step(80);

  if (digitalRead(6) == LOW)
    myStepper.step(-80);

  Blynk.run();
  timer.run(); // Initiates BlynkTimer
}
