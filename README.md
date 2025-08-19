# ESP-32-BLE-Controlled-Bot-V1.0
Flutter ESP32 BLE Cart Controller 🚗📱

Control a 2‑wheel cart driven by an H‑bridge (L298N/BTS7960) using an ESP32 over Bluetooth Low Energy and a Flutter joystick app.
The app talks to the ESP32 via the Nordic UART Service (NUS) with simple ASCII commands.

✨ Features

BLE scan & connect to ESP32-Cart

On‑screen joystick + quick arrow buttons

Max speed slider (0–255)

ESP32 firmware with 15 kHz PWM, 1s failsafe, and simple protocol

Works with L298N/L9110/BTS7960 drivers

📂 Repository Layout
ble_cart_controller/
├─ lib/main.dart                 # Flutter app (controller)
├─ firmware/ESP32_Cart.ino       # ESP32 Arduino sketch (BLE + PWM)
├─ android/ ...  ios/ ...        # Native projects
├─ pubspec.yaml
└─ test/widget_test.dart


If your firmware/ folder isn’t created yet, add it and place the .ino there.

📱 Flutter App
Requirements

Flutter 3.x

flutter_blue_plus and permission_handler (already in pubspec.yaml)

Android permissions (already included)

Check android/app/src/main/AndroidManifest.xml has:

<uses-feature android:name="android.hardware.bluetooth_le" android:required="true" />
<uses-permission android:name="android.permission.BLUETOOTH" />
<uses-permission android:name="android.permission.BLUETOOTH_ADMIN" />
<uses-permission android:name="android.permission.BLUETOOTH_SCAN" />
<uses-permission android:name="android.permission.BLUETOOTH_CONNECT" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

iOS permission (already included)

ios/Runner/Info.plist:

<key>NSBluetoothAlwaysUsageDescription</key>
<string>App uses Bluetooth to control your robot.</string>

Run
flutter clean
flutter pub get
flutter run

Disable debug banner

In MaterialApp:

debugShowCheckedModeBanner: false,

🧠 BLE Command Protocol (phone → ESP32)

Drive: D:<left>,<right>\n (each value in −255…255)

D:120,120 → forward

D:120,-120 → spin left

D:-200,-200 → reverse

Stop: S\n

Firmware stops the motors if no command for 1 second (failsafe).

🔌 ESP32 Firmware

Arduino IDE → Boards Manager → install esp32 by Espressif Systems

Library: ESP32 BLE Arduino (Kolban)

Sketch: firmware/ESP32_Cart.ino

Default pin map (L298N)
ESP32	L298N	Side
25	IN1	Left
26	IN2	Left
27	ENA	Left PWM
32	IN3	Right
33	IN4	Right
14	ENB	Right PWM

Power & wiring tips

Common GND between ESP32 and motor driver

Do not power motors from ESP32 3V3/5V

Add 470–1000 µF electrolytic cap on motor supply

Consider a separate buck converter for ESP32 5V

Using BTS7960/IBT‑2? Open an issue or PR with your pins; easy to adapt.

🧪 Quick Test

Flash the firmware; you should see a BLE device ESP32-Cart.

Run the Flutter app; tap to connect.

Move the joystick → wheels should respond.

Release joystick → app sends S\n (stops). If the phone disconnects, firmware also stops after 1s.

🛠 Troubleshooting

Nothing moves / resets randomly: separate motor & logic supplies; big cap; common ground.

Android 12+ no devices: grant Bluetooth Scan/Connect + Location (legacy).

BLE lag: MTU is set to 185 and commands at ~20 Hz; keep the phone close.

📸 Screenshots

Add screenshots to screenshots/ and reference them here:

![Scan Page](screenshots/scan.png)
![Controller](screenshots/controller.png)

🤝 Contributing

PRs welcome! Ideas:

Telemetry (battery/IMU) via NUS TX → Flutter HUD

PID speed control

Path recording & replay

BTS7960 pin presets

📄 License

MIT © 2025 megacrafttech

Create a LICENSE file with:

MIT License

Copyright (c) 2025 megacrafttech

Permission is hereby granted, free of charge, to any person obtaining a copy...

🚀 Publishing to GitHub
git init
git add .
git commit -m "Initial commit: Flutter ESP32 BLE Cart Controller"
git branch -M main
git remote add origin https://github.com/megacrafttech/ble_cart_controller.git
git push -u origin main
