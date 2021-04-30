# TsunKitTranslations
Locale/translation files for TsunKit/KCNav.

## Submission
Adding a new language is just a matter of copying one of the existing ones, changing the directory name, and modifying the JSON file with the new translations.

Additions to the translation files may be submitted through the standard fork/PR process. Alternatively, If you aren't familiar enough with git to go through the normal process, feel free to submit your proposed changes via the `Issues` page.

PRs are welcome for both new language additions and improvements to existing language files.

Submissions to this repo will be reviewed and pulled into the TsunKit preview instance at http://kcbeta.piro.moe/

### Directory naming
language directories should be named based on the locale string for the target language.

To find your current system locale string, in Chromium-based browsers:
1) Press`F12` or `Ctrl+Shift+I` to open your browser's developer tools
2) Go to the `Console` tab
3) Enter `navigator.language` into the console input and press `Enter`.

or, from Windows 10 PowerShell, run `Get-WinSystemLocale` and look under the `Name` column.

The locale string has a format like `en-US`, the first part being the primary locale and the second being the sub-locale.

For new language additions, the folder can be named based on either the primary locale alone (e.g. `en`) or the whole thing (e.g. `en-US`), depending on whether different translation sets for different sub-locales are desired.

## Format
The language file format is simple JSON, plus interpolated tokens. JSON translation files should be saved with Unicode encoding.

Each entry has an `identifier` which uniquely identifies that translation entry, and `translation` text which is what will be displayed when that identifier is referenced by TsunKit.

e.g.
```JSON
{
    "identifier1": "Translation 1",
    "identifier2": "Translation 2"
}
```

### Value references

TsunKit may, in some cases, provide values to be rendered as part of translated text. These are referenced in translation entries by tokens in `{{curly braces}}`.

e.g.
```JSON
    "classicLine": "There are {{count}} lights!"
```

The entry above might be rendered as `"There are 4 lights!"` after passing through the translation engine, assuming that `count` is a valid token and the value provided by TsunKit is `4`.

These tokens are specific to the individual entries; You may only use the ones that are already there, and cannot insert tokens from other entries and expect them to Just Work.

You can, however, format the translation around them in any way you wish, and in the case of entries with multiple token references, you may change the order in which they appear.

### Term references

Sometimes, it can be useful to define a 'term' in one location, and then reference it elsewhere. This allows you to do things like changing the word used for something in multiple places at once, with only a single change.

This can be confusing to explain, so it's best to use an example:
```JSON
    "termWhat": "World",
    "placeholderText": "Hello, [[termWhat]]!"
```
In the above example, an entry named `termWhat` is defined, containing the text `World`. This is a translation entry that wouldn't be referenced directly by TsunKit -- instead, it's intended to be referenced only by other translation entries!

There is another translation entry, named `placeholderText`, which contains a *reference* to `termWhat`.

When rendered by TsunKit, the `placeholderText` entry would display as `Hello, World!`.

You could reference `termWhat` in other entries as well, and if you ever changed the value of `termWhat`, all other entries that reference it would be updated automatically.

TsunKit's English translation files have a set of predefined entries intended to be used in this fashion, marked with the `term` prefix. These entries are not directly referenced from TsunKit itself, and they can be removed in other language files if they are no longer needed. New term entries may also be freely added if desired.



And that's about it!
