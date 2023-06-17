//Smart Parking using IoT


#include <Servo.h>
#include <LiquidCrystal.h>
#define IR1 7
#define IR2 8

#define ir_car1 3
#define ir_car2 4
#define ir_car3 5
#define ir_car4 6

const int rs = 12, en = 11, d4 = 10, d5 = 9, d6 = 2, d7 = 1;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);

int S1=0, S2=0, S3=0, S4=0;
int flag1=0, flag2=0; 
int slot = 4;


Servo myservo;  // create servo object to control a servo
// twelve servo objects can be created on most boards

int pos = 0;    // variable to store the servo position

void setup() {

  myservo.attach(13);  // attaches the servo on pin 9 to the servo object
  pinMode(IR1,INPUT);
  pinMode(IR2,INPUT);

  pinMode(ir_car1, INPUT);
  pinMode(ir_car2, INPUT);
  pinMode(ir_car3, INPUT);
  pinMode(ir_car4, INPUT);



  myservo.write(90);
  lcd.begin(16, 2);  
  lcd.setCursor (3,0);
  lcd.print("Car  Parking");
  lcd.setCursor (0,2);
  lcd.print("     System     ");
  delay (5000);
  lcd.clear();   

  Read_Sensor();

  int total = S1+S2+S3+S4;
  slot = slot-total;

}

void loop() {

  Read_Sensor();



lcd.setCursor (0,0);
if(S1==1){lcd.print("S1:Fill ");}
     else{lcd.print("S1:Empty");}

lcd.setCursor (8,0);
if(S2==1){lcd.print("S2:Fill ");}
     else{lcd.print("S2:Empty");}

lcd.setCursor (0,2);
if(S3==1){lcd.print("S3:Fill ");}
     else{lcd.print("S3:Empty");}

lcd.setCursor (8,2);
if(S4==1){lcd.print("S4:Fill ");}
     else{lcd.print("S4:Empty");}


  Serial.print("IR1");
  Serial.println(digitalRead(IR1));
  Serial.print("IR2");
  Serial.println(digitalRead(IR2));
  if ( digitalRead(IR1)==0 || digitalRead(IR2)==0 ){
   pos = 90;
   myservo.write(pos);
   pos =0;
   myservo.write(pos);
   delay(3000);
   pos = 90;
   myservo.write(pos); 
   // waits 15ms for the servo to reach the position
   }
//  else if ( digitalRead(IR1)==0 && digitalRead(IR2)==1) 
//  {
//   pos=90;
//   myservo.write(pos);
//   delay(2500);                       // waits 15ms for the servo to reach the position
//  }
// else if (  digitalRead(IR1)==1 && digitalRead(IR2)==0)
// {
//    pos = 0;
//    myservo.write(pos);
//    delay(2500);                       // waits 15ms for the servo to reach the position
//  myservo.write(pos);
//    }
//  else {
//  lcd.setCursor (0,0);
//  lcd.print("  Have Slot: "); 
//  lcd.print(slot);
//  lcd.print("    "); 
//  }
  }

  void Read_Sensor(){
S1=0, S2=0, S3=0, S4=0;

if (digitalRead(ir_car1)==0){
  S1=1;
}
if (digitalRead(ir_car2)==0){
  S2=1;
}
if (digitalRead(ir_car3)==0){
  S3=1;
}
if (digitalRead(ir_car4)==0){
  S4=1;
}
}
