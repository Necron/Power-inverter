
/********************************************
 * Necron
 * 10/30/2015
 * Arduino Power System
 ********************************************/
int var;
int d = 8;//62.5HZ
int idle = 10; //pulse in microseconds
int detect;//detection threshold
int mode;//select between inverter mode and variable frequency mode
void setup() {
  Serial.begin(9600);
  pinMode(3, OUTPUT);
  pinMode(8, OUTPUT);
  pinMode(9, OUTPUT);
  pinMode(4, OUTPUT);
  pinMode(5, OUTPUT);
  pinMode(6, OUTPUT);
  pinMode(10, INPUT);
  pinMode(A0, INPUT);
  pinMode(A1, INPUT);


}
void loop() {
  digitalWrite(3, HIGH);
  condition();
  mode = digitalRead(10);
  if (mode == HIGH) {
    variable();
  }
  else {
    power();
  }
}
void power() { //supply power 62.5HZ
  digitalWrite(8, LOW);
  digitalWrite(9, HIGH);
  delay(8);
  digitalWrite(8, HIGH);
  digitalWrite(9, LOW);
  delay(8);
}
void variable() { //vary the frequency of output using 10k potentiometer on pin A1 between 5v+ and GND
  int count = 0;
  var = analogRead(A1);
  if (var < 8) {
    var = 8;
  }
  for (count = 0; count < 1000; count++) {
    digitalWrite(9, HIGH);
    digitalWrite(8, LOW);
    delayMicroseconds(var);
    digitalWrite(9, LOW);
    digitalWrite(8, HIGH);

//    Serial.print("variable mode, detection value =  ");
//    Serial.println(var);//debug
  }
}
void condition() {
  detect = analogRead(A0);
  if (detect > 1000) {
    digitalWrite(6, HIGH);
    digitalWrite(5, LOW);
    digitalWrite(4, LOW);
  }
  if (detect < 1000 && detect > 800) {
    digitalWrite(6, LOW);
    digitalWrite(5, HIGH);
    digitalWrite(4, LOW);
  }
  if (detect < 800) {
    digitalWrite(6, LOW);
    digitalWrite(5, LOW);
    digitalWrite(4, HIGH);
  }
//  Serial.print("RUNMODE, detection value =  ");
//  Serial.println(detect);//debug
}
