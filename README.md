#include <BlockDriver.h>
#include <FreeStack.h>
#include <MinimumSerial.h>
#include <SdFat.h>
#include <SdFatConfig.h>
#include <SysCall.h>
#include <SFEMP3Shield.h>
#include <SFEMP3ShieldConfig.h>
#include <SFEMP3Shieldmainpage.h>
#include <SPI.h>
SdFat sd;
SFEMP3Shield MP3player; //mp3

const int trigPin = 5; 
const int echoPin = 4; 
const int light = 0; 
const uint8_t volume = 5;
long duration;
int distance; 
int peopleDistance; 
int value = 5;


void setup() {
pinMode(trigPin, OUTPUT); 
pinMode(echoPin, INPUT); 
pinMode(light, INPUT);
Serial.begin(9600); 
sd.begin(SD_SEL, SPI_HALF_SPEED);
MP3player.begin();
}

void loop() {

Serial.print ("distance:");
Serial.print (distance); 
Serial.print (" ");
Serial.print ("light:");
Serial.println(value);  

digitalWrite(trigPin, LOW);
delayMicroseconds(2);
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);

duration = pulseIn(echoPin, HIGH);
distance= duration*0.034/2;
peopleDistance= distance;
value = analogRead(light);
MP3player.setVolume(volume, volume);


if ((peopleDistance>10) && (peopleDistance<15)){
MP3player.playTrack(4);
delay(10000);
MP3player.stopTrack();
}

else if ((peopleDistance>25) && (peopleDistance<30)){
MP3player.playTrack(1);
delay(5000); // do nothing after this track
}

else if ((peopleDistance>50) && (peopleDistance<70)){
MP3player.playTrack(2);
delay(2000);
}

else if ((peopleDistance>300) && (peopleDistance<400)){
MP3player.playTrack(5);
delay(1000);
}

else if ((value>200) && (value<300)){ //photocell sensor
MP3player.playTrack(3);
delay(7000);
}

 
else{
MP3player.stopTrack(); //stop any sound
}
}
