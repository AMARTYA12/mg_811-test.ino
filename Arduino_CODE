#include <SoftwareSerial.h>
#include <SPI.h>
#include <SD.h>
char commandChar;
int flag = 0;
File myFile;

/************************Hardware Related Macros************************************/
#define         MG_PIN                       A1//efine which analog input channel you are going to use
#define         BOOL_PIN                     2
#define         DC_GAIN                      5   //define the DC gain of amplifier
 
/***********************Software Related Macros************************************/
#define         READ_SAMPLE_INTERVAL         (50)    //define how many samples you are going to take in normal operation
#define         READ_SAMPLE_TIMES            (5)     //define the time interval(in milisecond) between each samples in 
                                                     //normal operation
 
/**********************Application Related Macros**********************************/
//These two values differ from sensor to sensor. user should derermine this value.
#define         ZERO_POINT_VOLTAGE           3.01//define the output of the sensor in volts when the concentration of CO2 is 400PPM
#define         REACTION_VOLTGAE             0.7 //define the voltage drop of the sensor when move the sensor from air into 1000ppm CO2
 
/*****************************Globals***********************************************/
float CO2Curve[3]  =  {2.602,ZERO_POINT_VOLTAGE,(REACTION_VOLTGAE/(2.602-3))};   
                                                     //two points are taken from the curve. 
 int sensorValue;
 //with these two points, a line is formed which is
                                                     //"approximately equivalent" to the original curve.
  int maxValue=0;
int minValue=100;
SoftwareSerial bluetoothSerial(2,3);
 float volts; //lope}; point1: (lg400, 0.324), point2: (lg4000, 0.280) 
char c[10];
char Date[10];     
int incomingByte=0; //slope = ( reaction voltage ) / (log400 –log1000) 
float MGRead(int mg_pin)
{
    int i;
    float v=0;
 
    for (i=0;i<READ_SAMPLE_TIMES;i++) {
        v += analogRead(mg_pin);
        delay(READ_SAMPLE_INTERVAL);
    }
    v = (v/READ_SAMPLE_TIMES) *30/1024 ;     //change the factor for proper range
    return v;  
}

void setup()
{
   Serial.begin(9600);                              //UART setup, baudrate = 9600bps
    pinMode(BOOL_PIN, INPUT);                        //set pin to input
    digitalWrite(BOOL_PIN, HIGH);                    //turn on pullup resistors
  while (!Serial) ; // wait for serial port to connect. Needed for native USB
    Serial.println("start");
    bluetoothSerial.begin(9600);
 

    //This example uses HC-05 Bluetooth to communicate with an Android device.
    //Download bluetooth terminal from google play store, https://play.google.com/store/apps/details?id=Qwerty.BluetoothTerminal&hl=en
    //Pair and connect to 'HC-05', the default password for connection is '1234'.
    //You should see this message from your arduino on your android device
    Serial.println("Bluetooth On....");
  while (!Serial) {
; // wait for serial port to connect. Needed for native USB port only
}
Serial.print("Initializing SD card...");
if (!SD.begin(10)) {
Serial.println("initialization failed!");
while (1);
}
Serial.println("initialization done.");
// open the file. note that only one file can be open at a time,
// so you have to close this one before opening another.
myFile = SD.open("test.txt", FILE_WRITE);
// if the file opened okay, write to it:
if (myFile) {
Serial.print("Writing to test.txt...");

Serial.println("done.");
} else {
// if the file didn't open, print an error:
Serial.println("error opening test.txt");
}

  // Serial.print("MG-811 Demostration\n");                
}
 
void loop()
{
 
  if (bluetoothSerial.available() > 0) {
     
    incomingByte = bluetoothSerial.read();
    if (isDigit(incomingByte)) {  // tests if myChar is a digit

    int a= incomingByte;
    char Date =a;
    Serial.print(Date);
   
    }
    else if (isPunct(incomingByte)){
      int i=1;
      switch(i){
        case 1: Serial.print(" : Date");
                  Serial.print("\n ");
                  i++;
                  break;
        case 2 :Serial.print(" : Month");
                 Serial.print("\n ");
                 i++;
                 break;
        case 3: Serial.print(" : Year");
                  Serial.print("\n ");
                  break;
      }
    }
    else{
      Serial.print("\n ");
    }
    
      }
  
}
void data(){
  sensorValue=analogRead(A0);
  volts=MGRead(MG_PIN);
  sensorValue=map(sensorValue,80,130,350,650);
  volts=map(volts,1.0,10.0,350,800);
  Serial.println("sensor Value");
  Serial.println(sensorValue,DEC);
  Serial.println("volts");
  Serial.println(volts,DEC);
  myFile.println("VOLTS");
  myFile.println(volts);
  myFile.println("SENSORVALUE");
  myFile.println(sensorValue);
  bluetoothSerial.println("volts:");
  bluetoothSerial.println(volts);
  bluetoothSerial.println("SensorValue");
  bluetoothSerial.println(sensorValue);
  delay(100);
  

  
  }
  
  


/*****************************  MGRead *********************************************
Input:   mg_pin - analog channel
Output:  output of SEN-000007
Remarks: This function reads the output of SEN-000007
************************************************************************************/ 

 
