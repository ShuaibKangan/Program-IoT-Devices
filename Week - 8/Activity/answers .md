#include <wifi.h>

//wifi credential.
const char* WIFI_SSID = "OPPOHotspot";
const char* WIFI_PASSWORD = "Burg3r$>H0Td5Gz369";

//state LED pin configuration.
const int statusLed = 2;

void setup() {
  Serial.begin(115200);

  pinMode(statusLed, OUTPUT);
  digitalWrite(statusLed, LOW);


  //Run the one-shot Wi-fi connection procedure.
  connectWiFi();
}

void loop() {
  if (WiFi.status() == WL_CONNECTED) {
    digitalWrite(statusLed, HIGH);
  } else {
    digitalWrite(statusLed, LOW);
  }
}

// Bounded blocking wi-fi inititialization function.
void connectWiFi() {
  Serial.print("Connecting to the Wi-Fi: ");
  Serial.println(WIFI_SSID);

  //Explicitly set ESP32 to station mode.
  WiFi.mode(WIFI_STA);
  WiFi.begin(WIFI_SSID, WIFI_PASSWORD);

  int attempts = 0;
  const int maxAttempts = 20; //max number of attempts to connect to wifi network.

  while (WiFi.status() != WL_CONNECTED && attempts < maxAttempts) {
    delay(500);
    Serial.print("Attempting to connect: ");
    Serial.println(attempts + 1);
    attempts++;
  }

  if (WiFi.status() == WL_CONNECTED) {
    Serial.print("\nConnected!");
    Serial.println("IP: address: ");
    Serial.println(WiFi.localIP());
  } else {
    Serial.println("Failed to connect to Wi-Fi");

  }
}
