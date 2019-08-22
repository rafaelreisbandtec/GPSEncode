#include<SoftwareSerial.h>
#include<TinyGPS++.h>

SoftwareSerial SerialGPS(8, 9);
TinyGPSPlus GPS;

//float lat, lon, vel;
//unsigned short sat;

void setup(){
  Serial.begin(9600);
  SerialGPS.begin(9600);

  Serial.print("Buscando satélites...");
}

void loop(){
  while(SerialGPS.available()){
    if(GPS.encode(SerialGPS.read())){
      
      if (GPS.date.isValid())
      {
        Serial.print(GPS.date.month());
        Serial.print(F("/"));
        Serial.print(GPS.date.day());
        Serial.print(F("/"));
        Serial.println(GPS.date.year());
      }
      else
      {
        Serial.println(F("Data indisponível"));
      }

      if (GPS.time.isValid())
      {
        if (GPS.time.hour() < 10) Serial.print(F("0"));
        Serial.print(GPS.time.hour());
        Serial.print(F(":"));
        if (GPS.time.minute() < 10) Serial.print(F("0"));
        Serial.print(GPS.time.minute());
        Serial.print(F(":"));
        if (GPS.time.second() < 10) Serial.print(F("0"));
        Serial.print(GPS.time.second());
        Serial.println(F("."));
      }
      else
      {
        Serial.println(F("Hora indisponível"));
      }
      /*Serial.print("- Hora - ");
      Serial.print(hora / 1000000);
      Serial.print(":");
      Serial.print((hora % 1000000) / 10000);
      Serial.print(":");
      Serial.print((hora % 10000) / 100);                         NÃO FUNFA POR CAUSA DA BIBLIOTECA
      Serial.print(" -- ");

      Serial.print("- Data - ");
      Serial.print(data / 10000);
      Serial.print("/");
      Serial.print((data % 10000) / 100);
      Serial.print("/");
      Serial.print(data % 100);
      Serial.print("--");
      */

      /*Serial.print("Latitude: ");
      Serial.println(lat, 6);
      Serial.print("Longitude: ");
      Serial.println(lon, 6);
      */
      
      if (GPS.location.isValid())
      {
        Serial.print(F("Latitude: "));
        Serial.println(GPS.location.lat(), 6);
        Serial.print(F("Longitude: "));
        Serial.println(GPS.location.lng(), 6);
      }
      else
      {
        Serial.println(F("Localização indisponível"));
      }

      
      
      /*vel = GPS.f_speed_kmph();

      Serial.print("Velocidade: ");
      Serial.println(vel);

      sat = GPS.satellites();

        if(sat != TinyGPS::GPS_INVALID_SATELLITES){                NEM ISSO
          Serial.print("Satelites: ");
          Serial.println(sat);
        }
        */
    }
  }
}
