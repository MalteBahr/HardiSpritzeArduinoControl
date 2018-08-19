// Arduino Cerea Teilbreitenschaltung
// (C) 2016 by Andreas Ortner - office@gh-ortner.com
// Weitergabe an Dritte nur unter Erlaubniss von Andreas Ortner
// Version Spritze und Cerea!!


/*

Mechanisch  Cerea Ergebnis
1           0     0
0           0     0
1           1     1
0           1     0



 */


String cmd;
char nextChar;
String teilbreite;
int anztb ;
int i;
int sensorVal;
int lsensorVal;
int teilbreitenCerea[7] = {1};

// Werte von Kabel an oberen Relais (Wertebereich 0-1000 entsprechend 0-12V)
// Es wird angenommen dass ein Wert über 500 einer angeschaltenen Teilbreite entspricht
int teilbreitenMechanisch[7];
int teilbreitenMechanischAlt[7];

void setup() {
  Serial.begin(9600);
  pinMode(2, OUTPUT);   //Teilbreite 1 Pin 2
  pinMode(3, OUTPUT);
  pinMode(4, OUTPUT);
  pinMode(5, OUTPUT);
  pinMode(6, OUTPUT);
  pinMode(7, OUTPUT);
  pinMode(8, OUTPUT);  // Teilbreite 7 Pin 8
  pinMode(9, OUTPUT);  // Pin 9  Hydraulik Befehl
  //pinMode(10, INPUT_PULLUP);   // Marker ein oder aus

  digitalWrite(2, HIGH);  // alle Ports of 0 = HIGH Setzten
  digitalWrite(3, HIGH);
  digitalWrite(4, HIGH);
  digitalWrite(5, HIGH);
  digitalWrite(6, HIGH);
  digitalWrite(7, HIGH);
  digitalWrite(8, HIGH);
  digitalWrite(9, HIGH);
}

void loop()
{

  analogReadAll();

  for(int j; j < anztb; j++){
    if(teilbreitenMechanischAlt[j] > 500 && teilbreitenMechanisch[j] < 500 ) digitalWrite(j+2,HIGH);
    if(teilbreitenMechanischAlt[j] < 500 && teilbreitenMechanisch[j] > 500 ){
      if(teilbreitenCerea[j] == 1) digitalWrite(j+2,HIGH);
      if(teilbreitenCerea[j] == 0) digitalWrite(j+2,LOW);
    }
  }
  
  
  cmd = "";   // String leeren
  ser();      // Commando einleisen
  
  if (cmd.startsWith("@CEREA;")) {    // Wenn Cerea entdeckt wird mit ausführung starten
    cerea();
  }
  Serial.print(cmd);

  

//sensorVal = digitalRead(10);   // pin 10 Abfragen
//  if (sensorVal == HIGH && sensorVal != lsensorVal) {
//    Serial.print ("@SDOSE;1;0;0;0;0;0;0;0;0;0;0;END");    //wenn Kontakt frei Marc ein
// Serial.print ('\r');
// Serial.print ('\n');
// lsensorVal= sensorVal;
//
//  } else if (sensorVal == LOW && sensorVal != lsensorVal)
//  
//  {
// Serial.print ("@SDOSE;0;0;0;0;0;0;0;0;0;0;0;END");   //wenn Kontakt auf GND Marc aus
// Serial.print ('\r');
// Serial.print ('\n');
//  lsensorVal= sensorVal;
//
//  }


  
  
}

// Wert überprüfen!!!!!
void analogReadAll(){
  for(int j; j < anztb; j++){
    teilbreitenMechanischAlt[j] = teilbreitenMechanisch[j];
  }
  
  teilbreitenMechanisch[0] = analogRead(A7);
  teilbreitenMechanisch[1] = analogRead(A6);
  teilbreitenMechanisch[2] = analogRead(A5);
  teilbreitenMechanisch[3] = analogRead(A4);
  teilbreitenMechanisch[4] = analogRead(A0);
  teilbreitenMechanisch[5] = analogRead(A2);
  teilbreitenMechanisch[6] = analogRead(A1);
}

void cerea()   //String auswerten
{
   
  cmd.remove(0, 7); // @Cerea; entferenen

  int first = cmd.indexOf(';');   // erstem ; suchen
  int second = cmd.indexOf(';', first + 1 );  // zweiten ; suchen
  cmd.remove(0, second + 1); //geschwindigkeit und minus 1 entferene

  int ende = cmd.indexOf('END'); //sucht nach "end"
  anztb = (ende - 2) / 2; //anzahl teilbreiten ermitteln

  teilbreite = cmd.substring(0, ende - 3); //teilbreiten in einen neuen string

 

  int b = 0;
  for (i = 0; i < anztb * 2 ; i = i + 2) { //teilbreiten schalten
    if (teilbreite.substring(i, i + 1) == "1")  teilbreitenCerea[b]=1;       //Teilbreite einschalten
    if (teilbreite.substring(i, i + 1) == "0" ) teilbreitenCerea[b]=0;      //Teilbreite ausschalten
    b = b + 1;
  }

  for(i = 0; i < anztb; i++){
    if (teilbreitenMechanisch[i] > 500 && teilbreitenCerea[i] == 0)  digitalWrite(i+2, LOW); // Ergebnis = Relais ein, teilbreite aus
    if (teilbreitenMechanisch[i] > 500 && teilbreitenCerea[i] == 1)  digitalWrite(i+2, HIGH); // Ergebnis = Relais aus, Teilbreite ein
    if (teilbreitenMechanisch[i] < 500 && teilbreitenCerea[i] == 1)  digitalWrite(i+2, HIGH); // Ergebnis = Relais aus, Teilbreite aus
    if (teilbreitenMechanisch[i] < 500 && teilbreitenCerea[i] == 0)  digitalWrite(i+2, HIGH); // Ergebnis = Relais aus, Teilbreite aus
  }

  for (i = anztb + 1 ; i < 8; i++) { // restlichen teilbreiten aus
    digitalWrite(i + 1, HIGH);
  }
}

//
//void hidrau()
//{
//  int ende = cmd.indexOf('EN'); //sucht nach "end"
//  cmd.remove(0, ende - 3); // rest entfernen
//  
//  
//  if (cmd.substring(0, 1) == "1") {
//    digitalWrite(9, LOW);         //hyro ein  --> hier low mit high ersetzten falls hydro nicht gewünscht wird
//  }
//  if (cmd.substring(0, 1) == "0") {
//    digitalWrite(9, HIGH);       //hyro aus
//  }
//}


void ser()   // Serielle Schnittstelle bis Return lesen
{
  if ( Serial.available() >0){
    do {
      if (Serial.available() > 0)
      {
        nextChar = Serial.read();
        if (nextChar >= 32) {
          cmd += nextChar;
        }
      }
    } while (nextChar != 10);  //solange lesen bis zeilenende
  }
}
