# Music Player — Android

Lokalny odtwarzacz muzyczny na Androida zbudowany we Flutterze.

## Architektura

```
Clean Architecture + BLoC
Presentation → Domain → Data
                ↑
         IAudioPlayer (abstrakcja)
         LocalAudioAdapter (just_audio + audio_service)
```

## Stack

| Warstwa | Technologia |
|---|---|
| UI | Flutter + Material 3 |
| State | flutter_bloc |
| Audio | just_audio + audio_service |
| Baza danych | Drift (SQLite) |
| Skanowanie | on_audio_query (MediaStore) |
| Tagi ID3 | metadata_god |
| DI | get_it |

## Setup środowiska (od zera)

### 1. Zainstaluj Flutter

```bash
# Windows — przez winget
winget install Flutter.Flutter

# Lub pobierz ręcznie z https://flutter.dev/docs/get-started/install
```

### 2. Zainstaluj Android Studio

- Pobierz z https://developer.android.com/studio
- Zainstaluj Android SDK przez SDK Manager
- Zainstaluj Android SDK Command-line Tools

### 3. Zaakceptuj licencje Android

```bash
flutter doctor --android-licenses
```

### 4. Sprawdź środowisko

```bash
flutter doctor -v
# Wszystkie ✓ poza Chrome/iOS są OK dla Androida
```

### 5. Podłącz urządzenie lub uruchom emulator

```bash
# Lista dostępnych urządzeń
flutter devices

# Uruchom emulator z Android Studio: Device Manager → Create Device
```

## Uruchomienie projektu

```bash
# Zainstaluj zależności
flutter pub get

# Wygeneruj kod (Drift + Freezed)
dart run build_runner build --delete-conflicting-outputs

# Uruchom w trybie debug na podłączonym urządzeniu
flutter run

# Build APK (debug)
flutter build apk --debug

# Build APK (release)
flutter build apk --release
```

## Struktura plików

```
lib/
├── core/
│   ├── audio/i_audio_player.dart     ← Interfejs abstrakcji audio
│   └── di/injection.dart             ← Konfiguracja GetIt
├── domain/
│   ├── entities/                     ← Track, Playlist
│   ├── repositories/                 ← Interfejsy repozytoriów
│   └── usecases/                     ← Logika biznesowa
├── data/
│   ├── audio/local_audio_adapter.dart ← just_audio implementacja
│   ├── local/
│   │   ├── database/app_database.dart ← Drift schema + queries
│   │   └── scanner/library_scanner.dart ← MediaStore scan
│   └── repositories/music_repository.dart
└── presentation/
    ├── player/                       ← PlayerScreen + PlayerBloc
    ├── library/                      ← LibraryScreen + LibraryBloc
    └── shared/                       ← MiniPlayer, AlbumArt, Theme
```

## Uprawnienia Android

- `READ_MEDIA_AUDIO` (Android 13+) — dostęp do plików audio
- `READ_EXTERNAL_STORAGE` (Android ≤12) — dostęp do plików audio
- `FOREGROUND_SERVICE_MEDIA_PLAYBACK` — odtwarzanie w tle
- `POST_NOTIFICATIONS` — powiadomienie z odtwarzaczem
- `WAKE_LOCK` — nie zasypiaj podczas odtwarzania

## Następne kroki (po MVP)

- [ ] Widok albumów z gridview okładek
- [ ] Widok artystów
- [ ] Zarządzanie playlistami (tworzenie, edycja, reorder)
- [ ] Equalizer (just_audio equalizer plugin)
- [ ] Sleep timer
- [ ] Widgets na home screen (Glance API)
- [ ] Integracja Spotify (SpotifyAdapter implementuje IAudioPlayer)
- [ ] Export/import playlist M3U
- [ ] Edycja tagów ID3
