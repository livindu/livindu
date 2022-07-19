# Microcomputers Lab 04

<!-- If you have screenshots you'd like to share, include them here. -->


## INTRODUCTION

In this lab the main objective is to built a water level controlling system for a water tank using a PIC16F877A Microcontroller. Using the programmming techniques such as interrupts. Briefly the setup of the task is a water tank that has two DC motors to control the water flow within the tank where one DC motor is used to pump water into the tank and the other DC motor to pump water out of the tank. The water level of the tank is measured using three water level sensors which controls the two DC motors accordingly when to start the water inlet motor and when to start the water outlet motor. 


## OPERATIONAL DESIGN

![tank](https://user-images.githubusercontent.com/109481748/179643808-10cba064-6e39-42c6-8a82-20b72ba9719f.jpg)


## OPERATIONAL TABLE
![chart](https://user-images.githubusercontent.com/109481748/179644327-14d12de8-2b1f-41dd-a4ae-7d573c4222fb.jpg)


## MAIN COMPONENTS

- Two DC motors
- Three water level sensors
- PIC16F877A Microcontroller
- Resistors
- Three Latched Switches
- One Crystal Oscillator
- Two Capacitors


## PCB DESIGN

![pcb](https://user-images.githubusercontent.com/109481748/179644550-bda3bf97-812c-4898-b0cd-0de3fe59d9dc.jpg)


## REAL IMPLEMENTATION

![Example screenshot](./img/screenshot.png)


## Screenshots
![Example screenshot](./img/screenshot.png)
<!-- If you have screenshots you'd like to share, include them here. -->



## RESULTS

CASE 1: when the sensor 1 is ON the MOTOR 1 gets ON (MOTOR 2 OFF)
![Example screenshot](./img/screenshot.png)

CASE 2: when the sensor 1 and sensor 2 are ON the MOTOR 1 gets ON (MOTOR 2 OFF)
![Example screenshot](./img/screenshot.png)

CASE 3: when the sensor 1, sensor 2 and sensor 3 are ON the MOTOR 2 starts and runs for a time of 500ms and gets OFF (MOTOR 1 OFF)
![Example screenshot](./img/screenshot.png)


## CODE

<!-- CODE START -->


#pragma config FOSC = HS // Oscillator Selection bits (HS oscillator)

#pragma config WDTE = OFF // Watchdog Timer Enable bit (WDT disabled)

#pragma config PWRTE = OFF // Power-up Timer Enable bit (PWRT disabled)

#pragma config BOREN = OFF // Brown-out Reset Enable bit (BOR disabled)

#pragma config LVP = OFF // Low-Voltage (Single-Supply) In-Circuit Serial Programming Enable bit (RB3 is digital I/O, HV on MCLR must be used for programming)

#pragma config CPD = OFF // Data EEPROM Memory Code Protection bit (Data EEPROM code protection off)

#pragma config WRT = OFF // Flash Program Memory Write Enable bits (Write protection off; all program memory may be written to by EECON control)

#pragma config CP = OFF // Flash Program Memory Code Protection bit (Code protection off)

#define _XTAL_FREQ 20000000

#include <xc.h>

#include <xc.h>

//ISR SET INTERRUPT SERVICE ROUTINE

void __interrupt() isr(void) 
{
    if (INTF==1) //Check if the interrupt is On
    {
    INTF=0; //Clear the interrupt
    
// COMBINATION 3 OF THE OPERATION TABLE

   if(RB2==1 && RB1==1 && RB0==1){
       
    RC2=0; // Motor 1 is OFF
    RC1=1; // Motor 2 is ON
    __delay_ms(500);  //MOTOR 2 is kept ON for 500ms
    RC1=0;    //Motor 2 is off
    
     }
    }
}
      
    
   

void main(void)
{
 // PORT configuration for SWITCHES / SENSORS and MOTORS
    
   TRISB0 = 1; //Pin 0 of the PortB is the SWITCH 3
   TRISB1 = 1; //Pin 1 of the PortB is the SWITCH 2
   TRISB2 = 1; //Pin 2 of the PortB is the SWITCH 1
   TRISC2 = 0; //OUTPUT for MOTOR 1
   TRISC1 = 0; //Output for MOTOR 2
 
   INTF = 0;  
 
   RC2=0; //MOTOR 1 OFF
   RC1=0; //MOTOR 2 OFF
 
 
//OPTION_REG = 0b00000000;
   
   GIE=1;    //Enable global interrupt-Enables all unmasked interrupt
   PEIE=1;   //Peripheral Interrupt enable-Enables all unmasked peripheral interrupts
   INTE =1;  //enable RB0 interrupt -  Enables the RB0 external interrupt
 
 while(1)
 {
// COMBINATION 1 OF THE OPERATION TABLE   
     if (RB2==1 && RB1==0 && RB0==0 )
 {
    RC2=1; //Turn ON Motor 1
    
 } else {
     
// COMBINATION 2 OF THE OPERATION TABLE 
     if(RB2==1 && RB1==1 && RB0==0)
 {
   RC2=1;  //Turn ON Motor 1  
   
 } else {        
    RC2=0;  //Turn OFF Motor 1 since there is no condition than above, other than the interrupt 
 }
 } 
}
 return; 
}


<!-- CODE END -->

