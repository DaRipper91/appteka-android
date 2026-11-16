# AGENTS.md — appteka-android

## Project Overview
`appteka-android` is a free, open-source alternative Android application store and package manager client (`com.tomclaw.appsend`). It allows users to discover, download, inspect, share, and manage APKs with community interactions, ratings, and moderation tools.

- **Package Namespace:** `com.tomclaw.appsend`
- **Application Modules:**
  - `:app` — Primary Android application (MVP UI layer, OkHttp networking, Room/SQLite cache, Chucker inspection).
  - `:krop` — Lightweight Android image cropping library.

---

## Technical Stack & BOM
- **Language:** Kotlin 2.3.21
- **Android Gradle Plugin (AGP):** 9.2.1
- **Gradle:** 9.4.1 wrapper
- **KSP:** 2.3.7
- **Target SDK:** 36 (compileSdk 37, minSdkVersion 23)
- **Key Dependencies:** AndroidX (AppCompat 1.7.1, Core 1.19.0, Fragment, RecyclerView), OkHttp 4.x, Chucker 4.3.1, MaterialRatingBar, Glide.

---

## Development & Hardware Directives
- **Host Architecture Notice (Apple Silicon M1 / Fedora Asahi):**
  - Android Gradle Plugin ships x86_64 `aapt2` binaries. On Fedora Asahi (ARM64), execution through `muvm` / `binfmt_dispatcher` can produce diagnostic banner output on stdout/stderr during daemon handshake.
  - For Kotlin and Java compilation verification:
    ```bash
    ./gradlew :app:compileDebugKotlin :krop:compileDebugKotlin
    ```
  - For full testing and packaging:
    Run on native Android hardware via Termux ADB bridge, or on x86_64 CI/emulation environments.
- **Single Source of Truth:**
  `AGENTS.md` is canonical. `CLAUDE.md`, `GEMINI.md`, and `.cursorrules` are symlinks pointing directly to this file.

---

## Architectural & Verification Patterns
- **Architecture:** MVP (Model-View-Presenter) with clean separation of presenter lifecycle, DTOs, and view contracts.
- **Accessibility (a11y):** All UI elements must maintain minimum 48x48dp touch targets, structured TalkBack content descriptions, and avoid CPU-heavy decorative animations.
- **Verification Commands:**
  ```bash
  # Check tasks and configuration
  ./gradlew tasks --group=verification

  # Compile Kotlin debug sources
  ./gradlew :app:compileDebugKotlin

  # Run debug unit test suite (where AAPT2 is available)
  ./gradlew testDebugUnitTest
  ```
