# 💧 ClearDrop

**A mobile app that helps anyone keep track of, and understand, the water they use, right from a smartphone.**

*Vietnamese name: **Nước Trong** ("clear water"). Vietnamese README: [README.vi.md](README.vi.md).*

ClearDrop is designed for places with limited access to water-testing laboratories and technical knowledge: remote, rural, and mountainous communities. With only a phone, a person can photograph the reading on a measuring device, save it together with its location, ask an assistant when something is unclear, and read simple guides about water.

> ⚠️ **Important:** ClearDrop is a tool for monitoring and learning. It **does not replace official laboratory testing**, and it cannot tell you that water is safe to drink. If you suspect contaminated water, or anyone has health symptoms, contact your local health authority or water supplier.

---

## ✨ Features

The app has five tabs:

| Tab | What it does |
|---|---|
| 🏠 **Home** | Browse easy-to-read topics about water (pH, minerals in drinking water, and more), with search. |
| 🤖 **Assistant** | Chat about water quality. You can attach a photo of a water sample or of a measuring device. |
| 📷 **Scan** | Take a photo (or pick one from the gallery) of a measuring device. The app reads the text in the image and lets you correct it. |
| 🕘 **History** | Review saved scans, delete records, and view where each scan was taken on a map. |
| 📖 **Guide** | How to treat water (boiling, filtering, chlorine disinfection, UV), how to check color, pH, smell, and turbidity, and first aid for suspected poisoning. |

### How a scan works

1. Open **Scan** and photograph the measuring device (or choose an image from the gallery).
2. The image is sent to a server for **text recognition (OCR)**.
3. You review the result and **edit it** if anything was misread.
4. The app gets the **GPS location** where the photo was taken (if you allow it).
5. Save it. The record appears in **History** with its image, time, and location.
6. In **History**, open the map to see one scan, or all scans, as pins.

---

## 🧰 Tech stack

- **React Native 0.85** with **TypeScript**, for Android and iOS
- Navigation: React Navigation (bottom tabs and native stacks)
- Camera: `react-native-vision-camera`
- Location: `@react-native-community/geolocation`
- Maps: Leaflet and OpenStreetMap, shown inside `react-native-webview`
- Local storage: `@react-native-async-storage/async-storage`
- Networking: `axios`

The app interface is currently in **Vietnamese**.

---

## 🔌 Backend

The app needs a separate server for text recognition and for the assistant. **The server code is not included in this repository.** The app reads the server address from the `SERVER_URL` variable and calls two endpoints:

**`POST {SERVER_URL}/ocr`**
```json
// Request
{ "image_base64": "<base64-encoded image>" }
// Response
{ "text": "<recognized text>" }
```

**`POST {SERVER_URL}/chat`**
```json
// Request (image_url is optional)
{ "message": "<question>", "image_url": "data:image/jpeg;base64,..." }
// Response
{ "reply": "<answer>" }
```

---

## 🚀 Getting started

### Requirements

- Node.js **22.11 or newer**
- A working React Native environment ([official guide](https://reactnative.dev/docs/set-up-your-environment)): Android Studio for Android, Xcode and CocoaPods for iOS

### Steps

```sh
# 1. Install dependencies
npm install

# 2. Create a .env file in the project root with your server address
echo "SERVER_URL=https://your-server-address" > .env

# 3. Start Metro
npm start

# 4a. Run on Android (in another terminal)
npm run android

# 4b. Or run on iOS (install pods the first time)
bundle install
bundle exec pod install --project-directory=ios
npm run ios
```

> If you change `.env`, restart with `npm run start:fresh` to clear the cache.

### Permissions the app asks for

| Permission | Why |
|---|---|
| Camera | Photograph measuring devices |
| Photo library | Choose existing images |
| Location | Save where each scan was taken |
| Internet | Reach the OCR and assistant server, and load map tiles |

---

## 📌 Current limitations

- An **Internet connection is required** for text recognition, the assistant, and the map.
- OCR output is **raw text**. The app does not yet extract individual values (pH, TDS, temperature) or raise alerts.
- Scan history is stored **on the phone only** and is not synced to a server.
- The map **shows scan locations only**; it does not analyze or flag polluted areas.
- This repository does not include tests or data that measure OCR accuracy.

## 🗺️ Roadmap

- Extract individual readings from OCR text and compare them with reference thresholds
- Alerts for unusual readings
- Offline use
- Data sync and area-level summaries
- Additional languages, including ethnic minority languages in Vietnam

---

## 👤 Author

**Nguyen Tan Dat**, Vinh University High School for the Gifted, Nghe An, Vietnam.

This project is related to, but separate from, the research paper *"WaterOCR-AI++: A Low-Cost OCR- and AI-Assisted Framework for Smartphone-Based Water Quality Monitoring"*, which proposes a research framework for reading colorimetric test strips with a phone.
