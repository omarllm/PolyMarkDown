`com.example.polyglotpocket  (main)`
- Actual app code
`com.example.polyglotpocket (androidTest)` 
- Tests that run on a device/emulator, it contains instrumented tests. It needs the Android Environment, so they are executed on a real Android device or an emulator. It is used when you need to test activities/fragments, UI interactions, etc.
`com.example.polyglotpocket (test)` 
- Tests that run locally on the computer, it contains local unit tests. It doesn't depend on Android, it contains things like function validation, **words comparison**, etc.

Keep in mind:
- **Instrumented test**: tests the app inside an Android environment, usually on an emulator or physical device.
- **Local unit test**: tests a small piece of program logic on your development computer.