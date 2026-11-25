
name: Build Slot Analyzer Pro

on:
  workflow_dispatch:
  push:
    paths:
      - 'SlotAnalyzerPro-Android-Starter.zip'
      - '.github/workflows/android.yml'

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Download repository
        uses: actions/checkout@v4

      - name: Install Java
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Install Android SDK
        uses: android-actions/setup-android@v3

      - name: Install Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.11.1'

      - name: Install Android platform
        run: sdkmanager "platforms;android-35" "build-tools;35.0.0"

      - name: Extract project
        run: |
          unzip -q SlotAnalyzerPro-Android-Starter.zip -d extracted

      - name: Build APK
        run: |
          cd extracted/SlotAnalyzerPro
          gradle assembleDebug

      - name: Save APK
        uses: actions/upload-artifact@v4
        with:
          name: SlotAnalyzerPro-APK
          path: extracted/SlotAnalyzerPro/app/build/outputs/apk/debug/app-debug.apk
