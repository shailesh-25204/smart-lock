#include <LiquidCrystal.h>
#include <Keypad.h>
#include <Servo.h>

void backspace();
void Match();
void noMatch();


Servo myservo;
int pos = 0;

/*     BLYNK App Integration    */

#include <BlynkSimpleStream.h>
char auth[] = "xdHkD3q_9nnjGXau3mDyDR6NZlK0V4NR";

LiquidCrystal lcd(8, 7, 13, 12, 11, 10);

/*     CODE FOR CUSTOM CHARECTERS    */

byte Block[8] = {
  0b11111,
  0b11111,
  0b11111,
  0b11111,
  0b11111,
  0b11111,
  0b11111,
  0b11111
};
byte keyRing[8] = {
  0b01110,
  0b11011,
  0b10001,
  0b10001,
  0b11011,
  0b01110,
  0b00000,
  0b00000
};
byte keyrod[8] = {

  0b00000,
  0b11111,
  0b00000,
  0b11111,
  0b11111,
  0b00000,
  0b00000,
  0b00000
};
byte key_main[8] = {
  0b00000,
  0b11111,
  0b00000,
  0b11111,
  0b11111,
  0b11011,
  0b11001,

};

/*     CODE FOR KEYPAD    */

const int ROW_NUM = 4; //four rows
const int COLUMN_NUM = 3; //three columns

char keys[ROW_NUM][COLUMN_NUM] = {
  {'1', '2', '3'},
  {'4', '5', '6'},
  {'7', '8', '9'},
  {'*', '0', '#'}
};

byte pin_rows[ROW_NUM] = {A5, A4, A3, A2}; //connect to the row pinouts of the keypad
byte pin_column[COLUMN_NUM] = {4, 3, 2}; //connect to the column pinouts of the keypad

Keypad keypad = Keypad( makeKeymap(keys), pin_rows, pin_column, ROW_NUM, COLUMN_NUM );

const String password = "1234"; // change your password here
String input_password;

int i = 0;
int inp = 0;

void setup() {

  myservo.attach(6);
  myservo.write(0);

  lcd.createChar(0, Block);
  lcd.createChar(1, keyRing);
  lcd.createChar(2, keyrod);
  lcd.createChar(3, key_main);

  pinMode(6, OUTPUT);
  pinMode(5, OUTPUT);
  pinMode(9, OUTPUT);

  digitalWrite(9, LOW);
  digitalWrite(5, LOW);

  lcd.begin(16, 2);
  lcd.setCursor(5, 0 );
  lcd.print("HELLO!");
delay(200);
  while (i <= 15)
  {
    lcd.setCursor(i, 0);
    lcd.write((uint8_t)0);
    lcd.setCursor(15 - i, 1);
    lcd.write((uint8_t)0);
    delay(50);
    i++;
  }

  delay(200);
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Enter Key!");
  int k = 1;

  for (int j = 13; j < 16; j++)
  { lcd.setCursor(j, 0);
    lcd.write((uint8_t)k);
    k++;
  }
  lcd.setCursor(0, 1);

  /*     BLYNK App Integration    */

  Serial.begin(9600);
  Blynk.begin(auth, Serial);

}

void loop() {
  Blynk.run();
  lcd.blink();
  char key = keypad.getKey();
  if (key) {
    if (inp == 0)
    {
      lcd.setCursor(0, 1);
    }
    inp++;
    if (key == '*')
    {
      backspace();
    }

    else if (key == '#') {
      lcd.noBlink();
      if (password == input_password) 
      {
      Match();
      } 
      else 
      {
      noMatch();
      }
      
      input_password = ""; // clear input password
      lcd.setCursor(0, 0);
      lcd.print("Enter Key!");
      int k = 1;
      for (int j = 13; j < 16; j++)
      {

        lcd.setCursor(j, 0);
        lcd.write((uint8_t)k);
        k++;

      }
      lcd.setCursor(0, 1);
    }
    else {
      input_password += key; // append new character to input password string
      lcd.print("*");
    }

  }

}
void backspace()
{
  lcd.noBlink();
  lcd.setCursor(0, 1);
  lcd.print("               ");
  lcd.setCursor(0, 1);
  input_password.remove(input_password.length() - 1);
  for (int z = 0; z < input_password.length(); z++)
  {

    lcd.setCursor(z, 1);
    lcd.print("*");
  }

}
void Match()
{
  lcd.clear();
  digitalWrite(5, HIGH);
  lcd.print("Unlocking. . . ");
  for (pos = 0; pos <= 180; pos += 1)
  {
    myservo.write(pos);
    delay(10);
  }
  inp = 0;
  lcd.clear();
  Blynk.notify("Device has been Unlocked");  // BLYNK App Integration
  Blynk.email("shaileshdeshmukh25204@gmail.com","Smart Lock","Device Has Been Unlocked. Please Contact Security if you did not do this.");
 
  lcd.setCursor(0, 0);
  delay(3000);
  myservo.write(0);
  digitalWrite(5, LOW);
  lcd.print("Enter Key!");
  int k = 1;

  for (int j = 13; j < 16; j++)
  { lcd.setCursor(j, 0);
    lcd.write((uint8_t)k);
    k++;
  }
  lcd.setCursor(0, 1);

}
void noMatch()
{
lcd.clear();
  lcd.print("Try again...");
  for (int i = 0; i < 4; i++)
  {
    digitalWrite(9, HIGH);
    delay(500);
    digitalWrite(9, LOW);
    delay(500);
  }
lcd.clear();
  inp = 0;
  Blynk.notify("Attempt Failed"); //BLYNK App Integration
  Blynk.email("shaileshdeshmukh25204@gmail.com","Smart Lock","Atempt Failed. Please Contact Security if you did not do this.");
    lcd.print("Enter Key!");
  int k = 1;

  for (int j = 13; j < 16; j++)
  { lcd.setCursor(j, 0);
    lcd.write((uint8_t)k);
    k++;
  }
  lcd.setCursor(0, 1);

}
