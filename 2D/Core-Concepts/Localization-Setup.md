# Unity Localization and Accessibility Setup

Unity's Localization package supports translated strings, locale-specific assets, Smart Strings for values/plurals, pseudo-localization, and import/export workflows. Package availability and APIs depend on the installed Unity version.

Official Unity 6 manual: https://docs.unity3d.com/6000.0/Documentation/Manual/com.unity.localization.html

## Install and create tables
1. Install the Localization package through Package Manager.
2. Open the Localization Settings workflow and create a Localization Settings asset if prompted.
3. Add the locales your game needs (for example, English and another target language).
4. Create a String Table Collection such as UI.
5. Give entries stable keys such as menu.play, menu.settings, and game.pause.
6. Enter translations per locale instead of hardcoding visible sentences throughout scripts.
7. Add a Localize String Event or LocalizedString binding to UI elements supported by the chosen UI system.
8. For language selection, set the selected locale through the package's documented API and save the user's preference.

Exact menu locations and UI components can vary by package version. Follow the documentation for your installed package.

## Localized assets
The package can provide different assets for different locales. This is useful when art contains text, audio is dubbed, or a region requires a different graphic. Avoid embedding text inside images if it could instead be translated as text.

## Text layout
Translations may be much longer or shorter than the source. Test buttons, subtitles, dialogs, text wrapping, font glyph coverage, and right-to-left layouts if supported. Avoid hardcoded widths that only fit English.

## Accessibility basics
- Provide readable text size and strong contrast.
- Do not rely on color alone to indicate an important state.
- Add captions/subtitles where appropriate.
- Support control remapping where appropriate and give UI focus/navigation clear behavior.
- Test keyboard, controller, touch, and different screen sizes according to the target platform.

## Testing
Use pseudo-localization to reveal clipping early. Test every UI screen with each locale and inspect fallback behavior when an entry/asset is missing. Keep keys stable; change translations independently of code.
