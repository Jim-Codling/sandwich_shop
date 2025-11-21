# Sandwich Shop (flutter_application_1)

A small Flutter demo app for counting and ordering sandwiches. It demonstrates a simple UI with state management via a tiny repository, selection controls, and widget tests.

## Main features

- Increment / decrement sandwich quantity with bounds:
  - Prevents decrementing below 0 and incrementing above a configured max. See [`OrderRepository`](lib/repositories/order_repository.dart) for the logic.
- Toggle sandwich size between "six-inch" and "footlong". UI lives in [`OrderScreen`](lib/main.dart).
- Select bread type via a dropdown backed by the [`BreadType`](lib/main.dart) enum.
- Add a free-form order note via a `TextField`.
- Live order summary showing:
  - Quantity, bread type, size and a repeating sandwich emoji.
  - Order note.
  See [`OrderItemDisplay`](lib/main.dart) for the display widget.
- Reusable styled buttons via [`StyledButton`](lib/main.dart).

## How it works (quick overview)

- App entry: [lib/main.dart](lib/main.dart).
  - `App` builds a `MaterialApp` with `OrderScreen` as home.
  - `OrderScreen` (stateful) composes the UI and holds:
    - `_orderRepository` — an instance of [`OrderRepository`](lib/repositories/order_repository.dart) that tracks quantity and enforces `maxQuantity`.
    - `_isFootlong` boolean — controls size toggle.
    - `_selectedBreadType` — selected value of `BreadType`.
    - `_notesController` — `TextEditingController` for the notes input.
  - UI elements:
    - Size toggle: a `Switch` between "six-inch" and "footlong".
    - Bread selection: `DropdownMenu<BreadType>` constructed from the [`BreadType`](lib/main.dart) enum.
    - Notes: `TextField` with key `notes_textfield`.
    - Controls: `StyledButton` instances for Add/Remove wired to `_getIncreaseCallback()` and `_getDecreaseCallback()` which consult [`OrderRepository.canIncrement`](lib/repositories/order_repository.dart) and [`OrderRepository.canDecrement`](lib/repositories/order_repository.dart).
    - Summary: `OrderItemDisplay` shows the textual summary and emoji string computed as `'🥪' * quantity`.

- Business logic: See [lib/repositories/order_repository.dart](lib/repositories/order_repository.dart)
  - Class: [`OrderRepository`](lib/repositories/order_repository.dart)
    - Fields: `_quantity`, `maxQuantity`.
    - Methods: `increment()`, `decrement()`, and getters `quantity`, `canIncrement`, `canDecrement`.

## Files of interest

- App entry & UI:
  - [lib/main.dart](lib/main.dart) — contains `App`, `OrderScreen`, `StyledButton`, `OrderItemDisplay`, and `BreadType`.
    - [`OrderScreen`](lib/main.dart)
    - [`StyledButton`](lib/main.dart)
    - [`OrderItemDisplay`](lib/main.dart)
    - [`BreadType`](lib/main.dart)
- Repository / state:
  - [lib/repositories/order_repository.dart](lib/repositories/order_repository.dart) — [`OrderRepository`](lib/repositories/order_repository.dart)
- Tests:
  - [test/views/widget_test.dart](test/views/widget_test.dart) — widget tests that validate UI flows (add/remove, dropdown, notes, size toggle).

## Running the app

Make sure you have Flutter installed and the platform toolchains set up.

- Get packages:
  ```sh
  flutter pub get
  ```

- Run on connected device / emulator:
  ```sh
  flutter run
  ```

- Run a specific platform (example: Windows):
  ```sh
  flutter run -d windows
  ```

Configuration values and package references are in [pubspec.yaml](pubspec.yaml).

## Running tests

Run all tests with:
```sh
flutter test
```
See [test/views/widget_test.dart](test/views/widget_test.dart) for examples of UI tests and expectations.

## Extending the app

- Persist orders: add a storage layer and save recent orders.
- Add pricing & totals: compute price per size/bread and show order total.
- Add plugin support: Android / iOS / desktop embedder hooks are already scaffolded (see platform folders: `android/`, `ios/`, `windows/`, `linux/`, `macos/`).

## Notes

- This project was created from the Flutter template and includes platform scaffolding under `android/`, `ios/`, `windows/`, `linux/`, and `macos/`.
- For build configuration and platform-specific behavior see the respective platform files (for example, `android/app/build.gradle.kts`, `windows/CMakeLists.txt`).

License: Use as a learning sample. See the project files for more details.