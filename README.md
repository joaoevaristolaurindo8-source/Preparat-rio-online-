# Preparat-rio-online-unzip name: Compilar Preparatório Online

on:
  workflow_dispatch:

jobs:
  construir:
    runs-on: ubuntu-latest

    steps:
      - name: Obter projeto
        uses: actions/checkout@v4

      - name: Configurar Java 17
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'

      - name: Configurar Android SDK
        uses: android-actions/setup-android@v4
        with:
          packages: 'platform-tools platforms;android-35 build-tools;35.0.0'

      - name: Instalar Gradle
        uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: '8.7'

      - name: Criar Gradle Wrapper
        run: gradle wrapper --gradle-version 8.7

      - name: Compilar APK
        run: ./gradlew assembleRelease

      - name: Compilar AAB
        run: ./gradlew bundleRelease

      - name: Guardar arquivos
        uses: actions/upload-artifact@v4
        with:
          name: preparatorio-online
          path: |
            app/build/outputs/apk/release/*.apk
            app/build/outputs/bundle/release/*.aab
