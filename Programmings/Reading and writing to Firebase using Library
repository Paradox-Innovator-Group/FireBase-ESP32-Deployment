#include <Arduino.h>
#include <WiFi.h>
#include <Firebase_ESP_Client.h>

//Provide the token generation process info.
#include "addons/TokenHelper.h"
//Provide the RTDB payload printing info and other helper functions.
#include "addons/RTDBHelper.h"

// Insert your network credentials
#define WIFI_SSID "NT_OFFICE"
#define WIFI_PASSWORD "MajesticBLR@3214SH"

// Insert Firebase project API Key
#define API_KEY "AIzaSyA5jwgz-XIwALvBcFMxyK7KsvQ2kRiAM4o"

// Insert RTDB URLefine the RTDB URL */
#define DATABASE_URL "https://water-quality-fd458-default-rtdb.asia-southeast1.firebasedatabase.app/"

//Define Firebase Data object
FirebaseData fbdo, fbdo_s1, fbdo_s2, fbdo_s3, fbdo_s4, fbdo_s5, fbdo_s6, fbdo_status;

FirebaseAuth auth;
FirebaseConfig config;

unsigned long sendDataPrevMillis = 0;
int count = 0;
bool signupOK = false;

void setup() {
  Serial.begin(115200);
  WiFi.begin(WIFI_SSID, WIFI_PASSWORD);
  Serial.print("Connecting to Wi-Fi");
  while (WiFi.status() != WL_CONNECTED) {
    Serial.print(".");
    delay(300);
  }
  Serial.println();
  Serial.print("Connected with IP: ");
  Serial.println(WiFi.localIP());
  Serial.println();

  /* Assign the api key (required) */
  config.api_key = API_KEY;

  /* Assign the RTDB URL (required) */
  config.database_url = DATABASE_URL;

  /* Sign up */
  if (Firebase.signUp(&config, &auth, "", "")) {
    Serial.println("ok signup true");
    signupOK = true;
  }
  else {
    Serial.printf("%s\n", config.signer.signupError.message.c_str());
  }

  /* Assign the callback function for the long running token generation task */
  config.token_status_callback = tokenStatusCallback; //see addons/TokenHelper.h

  Firebase.begin(&config, &auth);
  Firebase.reconnectWiFi(true);

  if (!Firebase.RTDB.beginStream(&fbdo_s1, "/Sensor/Temperature"))
  {
    Serial.printf("Stream 1 Begin Error, %s\n\n", fbdo_s1.errorReason().c_str());
  }
  if (!Firebase.RTDB.beginStream(&fbdo_s2, "/Sensor/pH"))
  {
    Serial.printf("Stream 2 Begin Error, %s\n\n", fbdo_s2.errorReason().c_str());
  }
  if (!Firebase.RTDB.beginStream(&fbdo_s3, "/Sensor/TDS"))
  {
    Serial.printf("Stream 3 Begin Error, %s\n\n", fbdo_s3.errorReason().c_str());
  }
  if (!Firebase.RTDB.beginStream(&fbdo_s4, "/Sensor/Turbdity"))
  {
    Serial.printf("Stream 4 Begin Error, %s\n\n", fbdo_s4.errorReason().c_str());
  }
  if (!Firebase.RTDB.beginStream(&fbdo_s5, "/Sensor/EC"))
  {
    Serial.printf("Stream 5 Begin Error, %s\n\n", fbdo_s5.errorReason().c_str());
  }
  if (!Firebase.RTDB.beginStream(&fbdo_s6, "/Sensor/DO"))
  {
    Serial.printf("Stream 6 Begin Error, %s\n\n", fbdo_s6.errorReason().c_str());
  }
  if (!Firebase.RTDB.beginStream(&fbdo_status, "/Sensor/button"))
  {
    Serial.printf("Status Begin Error, %s\n\n", fbdo_status.errorReason().c_str());
  }
}
float ph = 0.0;
void loop() {

  //  if (Firebase.ready() && signupOK && (millis() - sendDataPrevMillis > 15000 || sendDataPrevMillis == 0)) {
  //    sendDataPrevMillis = millis();
  if (Firebase.ready() && signupOK)
  {
    if (!Firebase.RTDB.readStream(&fbdo_status))
      Serial.printf("button Error, %s\n\n", fbdo_status.errorReason().c_str());
    if (fbdo_status.streamAvailable())
    {
      if (Firebase.RTDB.getInt(&fbdo_status, "Sensor/button")) 
      {
        if (fbdo_status.dataType() == "int") {
          int fireStatus = fbdo_status.intData();
          Serial.println("Status from Firebase: " + fireStatus);


          if (fireStatus == 1)
          {
            if (Firebase.RTDB.setFloat(&fbdo, "Sensor/Temperature", count)) {
              //      Serial.println("PASSED");
              //      Serial.println("PATH: " + fbdo.dataPath());
              //      Serial.println("TYPE: " + fbdo.dataType());
            }
            else {
              Serial.println("FAILED to send Temperature");
              Serial.println("REASON: " + fbdo.errorReason());
            }
            count++;
            ph = 2.5; //random(0,100);
            // Write an Float number on the database path test/float
            if (Firebase.RTDB.setFloat(&fbdo, "Sensor/pH", 0.01 + ph)) {
              //      Serial.println("PASSED");
              //      Serial.println("PATH: " + fbdo.dataPath());
              //      Serial.println("TYPE: " + fbdo.dataType());
            }
            else {
              Serial.println("FAILED to send pH");
              Serial.println("REASON: " + fbdo.errorReason());
            }

            if (Firebase.RTDB.setFloat(&fbdo, "Sensor/TDS", 0.01 + random(0, 100))) {
              //      Serial.println("PASSED");
              //      Serial.println("PATH: " + fbdo.dataPath());
              //      Serial.println("TYPE: " + fbdo.dataType());
            }
            else {
              Serial.println("FAILED to send TDS");
              Serial.println("REASON: " + fbdo.errorReason());
            }
            if (Firebase.RTDB.setFloat(&fbdo, "Sensor/Turbidity", 0.01 + random(0, 100))) {
              //      Serial.println("PASSED");
              //      Serial.println("PATH: " + fbdo.dataPath());
              //      Serial.println("TYPE: " + fbdo.dataType());
            }
            else {
              Serial.println("FAILED to send Turbidity");
              Serial.println("REASON: " + fbdo.errorReason());
            }
            if (Firebase.RTDB.setFloat(&fbdo, "Sensor/EC", 0.01 + random(0, 100))) {
              //      Serial.println("PASSED");
              //      Serial.println("PATH: " + fbdo.dataPath());
              //      Serial.println("TYPE: " + fbdo.dataType());
            }
            else {
              Serial.println("FAILED to send EC");
              Serial.println("REASON: " + fbdo.errorReason());
            }
            if (Firebase.RTDB.setFloat(&fbdo, "Sensor/DO", 0.01 + random(0, 100))) {
              //      Serial.println("PASSED");
              //      Serial.println("PATH: " + fbdo.dataPath());
              //      Serial.println("TYPE: " + fbdo.dataType());
            }
            else {
              Serial.println("FAILED to send DO");
              Serial.println("REASON: " + fbdo.errorReason());
            }
          }
        }
      }
    }

  }
}

//    if (!Firebase.RTDB.readStream(&fbdo_s1))
//      Serial.printf("Stream 1 Error, %s\n\n", fbdo_s1.errorReason().c_str());
//    if (fbdo_s1.streamAvailable())
//    {
//      if (fbdo_s1.dataType() == "float")
//      {
//        Serial.println("Succesfully read from " + fbdo_s1.dataPath() + ":" + count + "(" + fbdo_s1.dataType() + ")");
//      }
//    }
//
//    if (!Firebase.RTDB.readStream(&fbdo_s2))
//      Serial.printf("Stream 2 Error, %s\n\n", fbdo_s2.errorReason().c_str());
//    if (fbdo_s2.streamAvailable())
//    {
//      if (fbdo_s2.dataType() == "float")
//      {
//        Serial.println("Succesfully read from " + fbdo_s2.dataPath() + ":" + ph + "(" + fbdo_s2.dataType() + ")");
//      }
//    }
