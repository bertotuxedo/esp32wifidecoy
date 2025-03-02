# esp32wifidecoy

# 🚀 ESP32 Wi-Fi Decoy Decoy

A simple **ESP32-WROOM-32U** project that **spoofs Wi-Fi access points** with randomly generated SSIDs and **random MAC addresses** every 45 seconds.

---

## 📌 Features

✔ **Spoofs fake Wi-Fi networks**  
✔ **Randomly changes SSID every 45 seconds**  
✔ **Generates a new random MAC address per SSID**  
✔ **Uses ESP32-WROOM-32U module**  
✔ **Runs autonomously after flashing**  

---

## 🛠️ Hardware Requirements

| Component            | Description |
|----------------------|-------------|
| **ESP32-WROOM-32U**  | Recommended for external antenna support |
| **Micro USB Cable** | Used to flash the ESP32 |
| **Arduino IDE** | Required for flashing |
| **Wi-Fi Scanner** | (e.g., WiFi Analyzer app for testing) |

---

## 📥 Installation Steps

### 1️⃣ Install Arduino IDE & ESP32 Board Support
1. **Download & Install Arduino IDE**  
   - [Download Here](https://www.arduino.cc/en/software)

2. **Install ESP32 Board Support**
   - Open **Arduino IDE → Preferences**  
   - Add this URL under **"Additional Board Manager URLs"**:
     ```
     https://dl.espressif.com/dl/package_esp32_index.json
     ```
   - Go to **Tools → Board → Board Manager**  
   - Search for **ESP32** and install **"ESP32 by Espressif Systems"**.

---

### 2️⃣ Install Required Libraries
🚀 **No additional libraries needed!**  
`WiFi.h` and `esp_wifi.h` are already included in the ESP32 framework.

---

### 3️⃣ Select the Correct Board
- **Board:** `ESP32-WROOM-DA Module`  
- **Partition Scheme:** `Huge APP (3MB No OTA)`  
- **Flash Mode:** `DIO`  
- **CPU Frequency:** `240 MHz`

---

### 4️⃣ Upload the Code
1. **Copy the Code Below** into **Arduino IDE**
2. **Select the correct board** (`ESP32-WROOM-DA Module`)
3. **Select the correct COM port** (under `Tools → Port`)
4. **Click Upload!** (Arrow Button)

---

## 📜 Code: ESP32 Wi-Fi Spoofer

```
#include <WiFi.h>
#include <esp_wifi.h>

#define WIFI_DURATION 45000  // Change SSID every 45 seconds

// Fake Wi-Fi SSIDs (Rotates Randomly)
const char* fakeSSIDs[] = {
    "Starlink_5G", "Free_Coffee_WiFi", "Xfinity_WiFi",
    "Hotel_Guest", "Public_Free_WiFi", "FBI_Surveillance_Van",
    "MetroLink_Internet", "McDonalds_Free_WiFi"
};
const int ssidCount = sizeof(fakeSSIDs) / sizeof(fakeSSIDs[0]);

void setup() {
    Serial.begin(115200);
    Serial.println("\n📡 Starting Fake Wi-Fi Access Point");
    randomSeed(analogRead(0));  // Seed for randomization
    switchToWiFi();
}

void loop() {
    delay(WIFI_DURATION);
    switchToWiFi();
}

// Function to Create a Fake Wi-Fi Network with Spoofed MAC
void switchToWiFi() {
    Serial.println("🔄 Switching Wi-Fi SSID...");
    
    WiFi.mode(WIFI_AP);
    uint8_t newMAC[6];
    generateRandomMAC(newMAC);
    esp_wifi_set_mac(WIFI_IF_AP, newMAC);  // Spoof MAC

    const char* ssid = fakeSSIDs[random(ssidCount)];
    WiFi.softAP(ssid, "12345678");  // Fake Password

    Serial.println("📡 Wi-Fi AP Active: " + String(ssid));
    Serial.printf("🕵️ Spoofed MAC: %02X:%02X:%02X:%02X:%02X:%02X\n", 
        newMAC[0], newMAC[1], newMAC[2], newMAC[3], newMAC[4], newMAC[5]);
}

// Function to Generate a Random MAC Address
void generateRandomMAC(uint8_t* mac) {
    for (int i = 0; i < 6; i++) {
        mac[i] = random(0x00, 0xFF);
    }
    mac[0] &= 0xFC;  // Ensure a valid, locally administered MAC
}
```
## 🛠️ How to Test
Open Serial Monitor (Set Baud Rate: 115200)

You should see output like this:

```
📡 Starting Fake Wi-Fi Access Point
🔄 Switching Wi-Fi SSID...
📡 Wi-Fi AP Active: FBI_Surveillance_Van
🕵️ Spoofed MAC: B4:06:CD:B8:C5:E1
Scan for Wi-Fi Networks on Your Phone/Laptop
```
Look for "FBI_Surveillance_Van", "Starlink_5G", etc.
It will rotate SSIDs every 45 seconds.
## ⚠️ Legal Disclaimer
This project is for educational and testing purposes only.
Using this tool in unauthorized environments may violate local laws and regulations.
🚨 Do not use this for malicious activities.

## 📌 FAQ
### ❓ Can I change the SSID rotation time?
Yes! Modify this line:

```
#define WIFI_DURATION 45000  // Change SSID every 45 seconds
```
Change 45000 to your preferred duration in milliseconds.

### ❓ Can I add more SSIDs?
Yes! Edit this list:

```
const char* fakeSSIDs[] = {
    "Starlink_5G", "Free_Coffee_WiFi", "NSA_Van_47", "Hidden_WiFi"
};
```
Just ensure ssidCount is updated.

### ❓ Can I use a different ESP32 model?
Yes, but ensure it supports SoftAP mode.

### ❓ How do I stop the script?
Simply unplug the ESP32 or upload a blank sketch.

## 📜 License
This project is licensed under the MIT License – Feel free to modify and share!

## 📡 Contributions
Want to improve the project? Fork it on GitHub and submit a pull request!
