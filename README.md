# PsyGuard AI

<p align="center">
  <img src="assets/icon.png" width="150" alt="PsyGuard AI Icon" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white" alt="Flutter" />
  <img src="https://img.shields.io/badge/Dart-0175C2?style=for-the-badge&logo=dart&logoColor=white" alt="Dart" />
  <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black" alt="JavaScript" />
  <img src="https://img.shields.io/badge/C++-00599C?style=for-the-badge&logo=c%2B%2B&logoColor=white" alt="C++" />
  <img src="https://img.shields.io/badge/CMake-064F8C?style=for-the-badge&logo=cmake&logoColor=white" alt="CMake" />
  <img src="https://img.shields.io/badge/Ruby-CC342D?style=for-the-badge&logo=ruby&logoColor=white" alt="Ruby" />
  <img src="https://img.shields.io/badge/Swift-F05138?style=for-the-badge&logo=swift&logoColor=white" alt="Swift" />
  <img src="https://img.shields.io/badge/SQLite-07405E?style=for-the-badge&logo=sqlite&logoColor=white" alt="SQLite" />
  <img src="https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white" alt="OpenAI" />
</p>

PsyGuard AI is a comprehensive mental health companion application built with Flutter. It serves as an MVP offering AI-powered companion chat, daily self-awareness check-ins, sleep tracking, risk observation, and trend analysis. The application prioritizes user privacy by storing data in a local SQLite database and utilizes an OpenAI-compatible API to facilitate the AI companion experience.

## Core Features

- **AI Companion Chat:** Engage with an AI designed to respond in a psychological counselor style. It dynamically adjusts its tone based on your emotional state—prioritizing empathetic listening when you are down, and offering analysis or actionable suggestions when your state is more stable.
- **Context Memory & Summarization:** The AI reads the history of the current chat session. To manage long conversations, it compresses older messages into concise summaries saved in the local database. It appends this context to recent messages so the AI "remembers" the ongoing discussion without exceeding token limits (e.g., nearing `128K`).
- **High-Risk Protection:** A built-in risk engine continuously monitors messages. If high-risk language is detected, the system immediately suspends general conversation and displays a dedicated safety protocol, guiding the user toward human support resources and crisis management.
- **Voice Interaction:** AI replies can be read aloud. The voice playback includes `Pause`, `Resume`, and `Stop` controls. Playback speed is fully customizable in the settings.
- **Daily Mindfulness Check-ins:** Users can record their daily mood, stress levels, energy, and additional notes.
- **Sleep Tracking:** Log sleep duration, bedtimes, and any difficulties experienced in falling asleep.
- **Data-Driven Trend Analysis:** The app generates charts and AI-driven reports to help users visualize and understand recent physical and mental changes. The home screen's state cards dynamically reflect real-time scores for mood, stress, and energy.
- **Localization:** Supports English and Traditional Chinese, easily switchable in the app settings (defaults to English). The AI companion dynamically adjusts its reply language to match the user's preference.
- **Local Data Privacy:** All chats, daily records, and risk snapshots are kept securely on the user's local device using SQLite.

## Tech Stack & Dependencies

- **Frontend Framework:** Flutter
- **State Management:** Riverpod (`flutter_riverpod`)
- **Routing:** GoRouter (`go_router`)
- **Database:** Drift + SQLite (`drift`, `drift_flutter`, `sqlite3_flutter_libs`)
- **Networking:** Dio (`dio`) for interacting with OpenAI-compatible `chat/completions` API
- **Voice & Accessibility:** `speech_to_text`, `flutter_tts`
- **Other Key Packages:** `flutter_dotenv`, `fl_chart`, `permission_handler`, `flutter_secure_storage`, `shared_preferences`, `flutter_markdown`

## Directory Structure

```text
.
├── lib
│   ├── app                # Application-wide setup and themes
│   ├── core               # Core utilities, networking, risk engine, safety, and storage
│   └── features           # Distinct feature modules
│       ├── chat           # AI companion chat interface and logic
│       ├── checkin        # Daily mindfulness tracking
│       ├── sleep          # Sleep tracking functionality
│       ├── safety         # High-risk detection and safety flows
│       ├── tools_library  # Assorted wellness tools
│       └── trends         # Charts and AI analysis reporting
├── assets                 # Static assets, fonts, icons
├── test                   # Unit and widget tests
├── integration_test       # End-to-end integration tests
└── pubspec.yaml           # Flutter dependencies and project configuration
```

## Local Development Workflow

Follow these steps to set up the project locally:

1. **Install Flutter Dependencies:**
   ```bash
   flutter pub get
   ```

2. **Environment Configuration:**
   Copy the example environment file:
   ```bash
   cp .env.example .env
   ```
   Open the `.env` file and populate the variables:
   ```env
   API_BASE_URL=https://api.openai.com
   API_KEY=your_api_key_here
   AI_MODEL=gpt-4o-mini
   APP_ENV=dev
   ```

3. **Code Generation:**
   Generate necessary boilerplate code for the Drift database and other build dependencies:
   ```bash
   dart run build_runner build --delete-conflicting-outputs
   ```
   *Note: Always re-run this command whenever you modify Drift tables or other code-generation dependent files.*

4. **Run Static Analysis & Tests:**
   ```bash
   flutter analyze
   flutter test
   ```
   *To run a single specific test file:*
   ```bash
   flutter test test/core/ai_chat_repository_test.dart
   ```

5. **Start the App:**
   ```bash
   flutter run
   ```

## Asset Generation

If you update the primary app icon in `assets/icon.png`, you need to regenerate the platform-specific icons using the `flutter_launcher_icons` package:
```bash
dart run flutter_launcher_icons
```

## Deployment Details

Currently, the verified processes in this repository cover local development, testing, and building for Flutter Web. There is no automated Coolify deployment flow verified for this project yet.

If you plan to deploy the Flutter Web build:
1. Ensure `web/sqlite3.wasm` and `web/drift_worker.js` are present in your project.
2. Build the web bundle:
   ```bash
   flutter build web
   ```
3. Publish the `build/web` directory to any static web hosting service. 
4. **Security Note:** If connecting to a cloud API from the web, ensure your deployment platform securely injects or manages the `.env` variables to prevent exposing sensitive API keys.

## FAQ

**Why can the AI remember previous content?**
Before your chat is sent, the system queries the local database for historical messages from that specific session. If the conversation context is nearing the token limit, the app automatically organizes older content into a summary. This summary is injected into future interactions so the AI retains a holistic understanding of the chat history without overflowing the context window.

**Why don't high-risk messages continue in the general chat?**
When the risk engine flags a message as high-risk, the system prioritizes the user's immediate safety. It intercepts the normal AI flow to present dedicated safety guidance and direct human support resources, which is a critical standard of care that supersedes casual companionship.
