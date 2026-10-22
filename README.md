# Gacha

A Flutter UI study of an animated "lucky draw" (gacha) screen, in the style of mobile
game reward screens. The screen title is Turkish: *Şanslı Çekiliş* ("Lucky Draw").

This is a UI and animation demo. All data is hard-coded mock data, there is no backend,
and spins don't award anything or change any values.

## What's on the screen

- **HUD:** the title, a help icon, and a currency bar showing a fixed amount (1000).
  The bar has a "+" buy button that does nothing yet.
- **Chest progress row:** a "collected / max" counter, with chest icons placed at each
  milestone. A chest turns grayscale once its milestone is reached.
- **Reward progress bar:** a custom gradient bar (`GameLinearBar`) with numbered
  milestone circles (40, 80, 120 and 160). The circles light up once the current points
  pass them.
- **Spin grid:** a 4×4 board. The 12 outer cells hold item cards, and the centre 2×2
  shows a gift box framed by rows of glowing dots. The board uses inset shadows from
  `flutter_inset_shadow`.
- **Spin buttons:** *Spin* and *Spin ×10*.

### Spin animation

`GridStateNotifier.startSpinningAndSelect()` moves a highlight clockwise around the 12
outer cells:

1. **Fast phase:** three full laps at 70 ms per step, starting from the previous result.
2. **Slow phase:** 300 ms per step until it reaches a randomly chosen target cell.
3. **Result:** the selected cell scales up with an `AnimationController` and gets a
   gradient border and glow.

A cooldown blocks a new spin until about 1 second after the result. *Spin ×10* runs
the sequence ten times in a row.

## Tech stack

- Flutter, Dart SDK `^3.8.1`
- State management: [`provider`](https://pub.dev/packages/provider)
  (`ChangeNotifierProvider` + `GridStateNotifier`)
- UI packages: `flutter_inset_shadow` for inset box shadows and `gradient_borders`
  for the gradient cell borders
- Lints: [`lint`](https://pub.dev/packages/lint) (`strict.yaml`)
- Image assets in `assets/`: background, cards, chest, gift, lights and the button
  artwork

## Project structure

All the code lives in `lib/`, one widget or class per file:

| File | Purpose |
|---|---|
| `main.dart` | Sets up `ChangeNotifierProvider<GridStateNotifier>` and `MaterialApp` |
| `grid_state_notifier.dart` | Spin sequence, highlighted and selected cell, points, milestones and chest counters |
| `gacha_page.dart` | Page layout and spin buttons |
| `animated_grid_box_with_provider.dart` | 4×4 board, outer cell layout and centre gift box |
| `grid_item.dart`, `glow_container.dart` | One cell, with its scale animation and glow/border styling |
| `gift_box_row.dart`, `reward_progress_bar.dart`, `game_linear_bar.dart` | Chest row and milestone progress bar |
| `gacha_hud.dart`, `currency_bar_hud.dart` | Title bar and currency display |
| `dots_row.dart`, `gray_scale_filter.dart`, `app_colors.dart` | Decorative dots, grayscale `ColorFilter` and colour palette |
| `game_item.dart`, `get_mock_game_items.dart` | Item model and mock items |

## Getting started

```bash
flutter pub get
flutter run
```

The project has platform folders for Android, iOS, web, Linux, macOS and Windows.

```bash
flutter analyze
```
