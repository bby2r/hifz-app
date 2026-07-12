# Хифз — Куран жаттоо колдонмосу (Android / Capacitor)

Офлайн-приложение для заучивания Корана. Весь арабский текст Корана и киргизский
перевод (Ш. Хакимов) встроены прямо в `www/index.html` и работают **без интернета**.
Интернет нужен только для аудио-декламации (кыраат), которая берётся с
`https://cdn.islamic.network`.

Android-обёртка собрана с помощью **Capacitor 8**.

## Что внутри

- `www/` — само веб-приложение (источник истины):
  - `index.html` — приложение целиком (≈3 МБ, текст Корана + перевод внутри файла).
    **Скопирован байт-в-байт, не изменялся и не минифицировался.**
  - `manifest.webmanifest`, `sw.js`, `icon-192.png`, `icon-512.png`, `OKUU.txt`
- `android/` — нативный проект Android (Capacitor)
- `assets/logo.png` — источник для иконки/сплэша (1024×1024, из `icon-512.png`)
- `capacitor.config.json` — конфигурация Capacitor

## Параметры приложения

|              |                       |
| ------------ | --------------------- |
| Название     | **Хифз**              |
| App ID       | `app.hifz.quran`      |
| webDir       | `www`                 |
| Ориентация   | только портрет        |
| minSdk / targetSdk | 24 / 36         |

## Что настроено в Android-обёртке

- **INTERNET** — разрешение в `AndroidManifest.xml` (для аудио с CDN).
- **Портретная ориентация** — `android:screenOrientation="portrait"`.
- **Авто-воспроизведение аудио** в WebView — в `MainActivity.java` вызывается
  `setMediaPlaybackRequiresUserGesture(false)`, чтобы аяты проигрывались и
  переключались автоматически.
- **keepScreenOn** — экран не гаснет во время заучивания.
- **Иконка** — adaptive icon (foreground + background) из `icon-512.png`, фон `#0c1a20`.
- **Splash** — фон `#0c1a20`.
- Все ссылки в `index.html` остались относительными и не менялись.

## Требования для сборки

- Node.js + npm
- JDK **21** (Capacitor 8 компилирует под Java 21)
- Android SDK: `platform-tools`, `platforms;android-36`, `build-tools;36.0.0`

## Сборка APK

```bash
npm install
npx cap sync android        # копирует www/ в android и синхронизирует нативный слой
cd android && ./gradlew assembleDebug
```

Готовый файл: `android/app/build/outputs/apk/debug/app-debug.apk`
Собранный debug-APK также прикреплён к [релизу](../../releases).

## Проверено

- **Аяты отображаются** — арабский текст + киргизский перевод.
- **По нажатию ▶️ идёт аудио** с `cdn.islamic.network`; аяты автоматически
  переключаются (1 → 2 → 3 …).
- `index.html` внутри APK **побайтово совпадает** с оригиналом (SHA-256
  `263ff1bd…96f3`).
