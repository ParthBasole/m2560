# This repo contains libraries for AVR ATmega 2560 & similar micro-controllers. It contains function names same as that used in Arduino IDE

io.h is the main library containing all the basic functions for ATmega2560 microcontroller
INDEX:
1]pulseIn()

1]PulseIn():

FUNCTIONS:
1.	unsigned long pulseIn();
  	This function reads a pulse (either HIGH or LOW) on a pin. For example, if value is HIGH, pulse_In() waits for the pin to go from       LOW to HIGH, starts timing, then waits for the pin to go LOW and stops timing. Returns the length of the pulse in microseconds or       gives up and returns 0 if no complete pulse was received within the timeout.

SYNTAX:
       		unsigned long pulseIn (volatile uint8_t bitno, uint8_t stateMask);

PARAMETERS:
    •	bitno: The pin number on which you want to read the pulse. (int)
    •	stateMask: type of pulse to read: either 1(for high) or 0(for low). (int)

2.	unsigned long microsecondsToInches();
    •	There are 73.746 microseconds per inch (i.e. sound travels at 1130 feet per second).This gives the distance travelled by the ping,       outbound and return, so we divide by 2 to get the distance of the obstacle.

    Formula: microseconds1*0.00669/ 2
    SYNTAX:
    unsigned long microsecondsToInches(unsigned long microseconds1)



3.	unsigned long microsecondsToCentimeters();
    •	The speed of sound is 340 m/s or 29 microseconds per centimetre. The ping travels out and back, so to find the distance of the           object we take half of the distance travelled.      
      Formula: microseconds*0.17/ 2
      SYNTAX:
      unsigned long microsecondsToCentimeters(unsigned long microseconds)
      
EXAMPLE CODE:
#include <avr/io.h>
#include <util/delay.h>
#include <pulseIn.h>


int main()
{
DDRC=0b11111111;
DDRD=0b00000000;
while(1) {
  // establish variables for duration of the ping, and the distance result
  // in inches and centimeters:
  unsigned long duration, inches, cm;

  // The PING))) is triggered by a HIGH pulse of 2 or more microseconds.
  // Give a short LOW pulse beforehand to ensure a clean HIGH pulse:
  //trig 10
  PORTC=(1<<pingPin);
  _delay_us(2);
  PORTC=(1<<pingPin);
  _delay_us(5);
 PORTC=(1<<pingPin);

  // The same pin is used to read the signal from the PING))): a HIGH pulse
  // whose duration is the time (in microseconds) from the sending of the ping
  // to the reception of its echo off of an object.
  //echo11
  duration = pulseIn(Pin,1);

  // convert the time into a distance
  
  inches = microsecondsToInches(duration);
  cm = microsecondsToCentimeters(duration);
  PORTB=0;
  if(cm<10)
  {
	  PORTB=0b00000001;	  
  }
  if(cm>=10||cm<20)
  {
	  PORTB=0b00000010;	  
  }
  if(cm>=20||cm<30)
  {
	  PORTB=0b00000100;	  
  }
  else if(cm>=30)
  {
	  PORTB=0b00001000;	  
  }
}
}
