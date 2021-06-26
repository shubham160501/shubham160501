#include <LiquidCrystal_I2C.h>
#define Sensor1 6
#define Sensor2 7
#define ControlLed 8
#define ON LOW
#define OFF HIGH
LiquidCrystal_I2C lcd(0x27, 16, 2);
int counter;
void Init_LCD(){
     lcd.init();       // Initialize the lcd
     lcd.backlight();  // Turn on LCD's led
     lcd.clear();
}
void Init_Sensor(){
  pinMode(Sensor1,INPUT_PULLUP); // Using internal resistor PULLUP
  pinMode(Sensor2,INPUT_PULLUP); // Using internal resistor PULLUP
}
  void Init_Control(){
  pinMode(ControlLed,OUTPUT); // OUPUT to control light
  digitalWrite(ControlLed, OFF); // Set off light     when init the system
}  
void LCD_display(){ 

    lcd.setCursor(0,0);   
    lcd.print("Person in room:");
    lcd.setCursor(0,1);
    lcd.print(counter);  
}

void Control(){
         if (counter > 0)
             digitalWrite(ControlLed, ON);
       else 
             digitalWrite(ControlLed, OFF);  
}
  void setup() {
       Serial.begin(9600); 
       // Init the system
       Init_LCD();
       Init_Sensor();
        Init_Control();
}
void loop() {

    LCD_display();
    Control();
     // Check sensor 1, if sensor 1 is covered
     if(digitalRead(Sensor1) == 0)
  {
 while(digitalRead(Sensor1) == 0); // Loop until  sensor 1 is uncovered 
 //delay(100); //We can put the delay here to   avoid people doesn't cover the sensor2 right-after sensor 1 is uncovered
 if(digitalRead(Sensor2) == 0) // If sensor 2 is covered right-after sensor 1 is uncovered
 {
  while(digitalRead(Sensor2)==0);// Loop until sensor 2 is uncovered
          counter++;
          lcd.clear();
          lcd.setCursor(0,0);   
          lcd.print("Person in room:");
          lcd.setCursor(0,1);
          lcd.print(counter); // Increase counter
        }
    }

     // Decrease also like that
   else if (digitalRead(Sensor2) == 0)
  {
       while(digitalRead(Sensor2) == 0);
      if(digitalRead(Sensor1) == 0)
    {
       while(digitalRead(Sensor1)==0);
          if(counter != 0)
            {
                 
                  counter--;
                  lcd.clear();
                  lcd.setCursor(0,0);   
                  lcd.print("Person in room:");
                  lcd.setCursor(0,1);
                  lcd.print(counter);  
                  }
           }
    }
}

