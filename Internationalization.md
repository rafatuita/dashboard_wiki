Based on current browser locale Dashboard can be displayed in one of the languages that it supports. Currently, list of languages includes Japanese and Chinese.

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