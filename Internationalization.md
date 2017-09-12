Based on current browser locale Dashboard can be displayed in one of the languages that it supports. List of currently supported languages includes Japanese, Chinese, and Chinese Traditional.

From developer perspective localization is handled by Google Closure Compiler's `goog.getMsg()` primitive. It allows the developer to define text that needs to be localized as simple variables anywhere in the code. The localization process itself is integrated into the build pipeline and backend component of Dashboard and happens automatically. Apart from placing new text into `MSG_` variables or `[[Message|]]` pattern and using those in the Angular templates, the developer is not required to do anything else.

## Translations

All translation data is stored in `i18n` directory in project's root. It includes `locale_conf.json` configuration file and translation files for each language, i.e. `messages-ja.xtb` or `messages-zh.xtb`.

### Introducing new language

To provide translations for a new language following steps have to be followed.

#### Update configuration file

Update `locale_conf.json` file to add new entry. Assuming following configuration:

```json
{
  "translations": [
    {"file": "messages-en.xtb", "key": "en"},
    {"file": "messages-zh.xtb", "key": "zh"},
    {"file": "messages-ja.xtb", "key": "ja"}
  ]
}
```

Following entry should be added:


```json
{"file": "messages-es.xtb", "key": "es"}
```

So configuration should look like this:

```json
{
  "translations": [
    {"file": "messages-en.xtb", "key": "en"},
    {"file": "messages-zh.xtb", "key": "zh"},
    {"file": "messages-ja.xtb", "key": "ja"},
    {"file": "messages-es.xtb", "key": "es"}
  ]
}
```

#### Generating translation files

To generate translation files run:

```sh
gulp generate-xtbs
```

As a result new translation file should appear in `i18n` directory.

### Translation procedure

To provide translations for specific language you should open one of the translation files and replace its English content in `<translation>` markup elements. Assuming file is being translated to Spanish following element in translation file:

```xml
<translation id="0" key="MSG_WELCOME" desc="Welcome message.">Welcome</translation>
```

Should be updated to:

```xml
<translation id="0" key="MSG_WELCOME" desc="Welcome message.">Bienvenido</translation>
```