name: Build APK

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2

    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: "3.9"

    - name: Install dependencies
      run: |
        sudo apt update
        sudo apt install -y git zip unzip openjdk-17-jdk python3-pip
        pip install --upgrade pip
        pip install buildozer

    - name: Build APK
      run: |
        buildozer init
        cp buildozer.spec original_buildozer.spec
        cp main.py main.py
        buildozer -v android debug

    - name: Upload APK
      uses: actions/upload-artifact@v3
      with:
        name: shadow-app-apk
        path: bin/*.apk
