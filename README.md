# Encore for Android

Native Android port of [Encore: Concert Diary](https://apps.apple.com/app/id6748657647), the iOS concert scrapbook app. Built with Devin bots, every diff reviewed by hand before it merges.

Status: planning. This repo holds the architecture and the day one plan; code lands as PRs.

## Module layout

Gradle multi-module, Kotlin, version catalog.

- `app`: single activity, Compose Navigation, Hilt setup
- `core/model`: plain Kotlin data classes (concert, artist, venue, setlist, media ref)
- `core/database`: Room entities, DAOs, migrations, exported schema checked into git
- `core/media`: local-only photo and video import through the photo picker, copied into app storage and never uploaded
- `core/designsystem`: dark theme, type and color tokens matching the iOS app
- `feature/diary`, `feature/concert-detail`, `feature/add-concert`, `feature/stats`

Each feature has a UI layer, a ViewModel and a repository interface.

## Architecture

- Unidirectional data flow with MVVM. Room exposes Flows, the repository maps them to domain models, the ViewModel combines them into one immutable `UiState` in a `StateFlow`, and Compose collects it with `collectAsStateWithLifecycle`.
- User actions go in as a sealed `Action` type. One-off effects (snackbars, navigation) go through a `Channel`.
- Coroutines and Flow throughout, Hilt for dependency injection.
- Offline first and local only: no backend and no sync layer to begin with.

## Day one tasks

Each task is its own PR with tests.

1. Scaffold the modules, version catalog, Hilt, Compose Navigation, and CI on GitHub Actions running lint, ktlint, unit tests and `assembleDebug`.
2. Room schema for concert, artist and venue, with DAO tests on an in-memory database.
3. Design system tokens and the dark theme.
4. Diary list screen: ViewModel with Turbine tests on the `UiState`, plus an empty state.
5. Add concert flow: form state and validation in the ViewModel, with unit tests.
6. Parity checklist listing every iOS feature, one issue each, ticked off as they ship.

A Google Play internal beta follows once the diary, the detail screen and adding a concert work end to end.
