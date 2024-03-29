Based on current browser locale the Dashboard can be displayed in one of the supported languages listed below. In case it does not work make sure that your browsers' locale is identified with correct language code.

| Language          | Code          |
| -------------     | ------------- |
| English (default) | en            |
| Chinese           | zh            |
| Chinese (Taiwan)  | zh-tw         |
| Japanese          | ja            |

From developer perspective localization is handled by [Google Closure Compiler's](https://github.com/google/closure-compiler) `goog.getMsg()` primitive. It allows the developer to define text that needs to be localized as simple variables anywhere in the code. The localization process itself is integrated into the build pipeline and backend component of Dashboard and happens automatically. Apart from placing new text into `MSG_` variables or `[[Message|Description]]` pattern and using those in the Angular templates, the developer is not required to do anything else.

## Introducing new localizable text

An explanation of the proper way of introducing a new localizable test to the Dashboard. There are two ways to do so.

### Localization with `MSG_` variables

Here is an example of a single `MSG_` variable definition:

```{js}
/** @export {string} @desc The text appears when the image pull secret name exceeds the maximal length. */
let MSG_IMAGE_PULL_SECRET_NAME_LENGTH_WARNING =
    goog.getMsg('Name must be up to {$maxLength} characters long.', {maxLength: '253'});
```

#### Guidelines
* Consistently name the object containing the variables for a given controller `i18n`.
* All variable names *must* start with `MSG_`.
* In the variable's name, after `MSG_`, try to write down the name of the controller (or part of Dashboard) to indicate where the variable is being used.
* The suffix of the name should indicate the type (or role) of the text-resource in the Dashboard (check the table below).
* Placeholders (`maxLength` above) are parts of the text that should remain the same in all translations.
* The description of each variable (`@desc` above) should describe the context of its usage. It is supposed to help the translator.
* The JavaDoc, containing all the annotations, should be kept at 1 line to reduce scrolling time.

| Suffix       | Usage                                                                   |
|--------------|:------------------------------------------------------------------------|
| _TITLE       | Title at the top of a window/view                                       |
| _SUBTITLE    | Subtitle placed directly beneath the title                              |
| _LABEL       | Text used as a temporary placeholder or to name an input field          |
| _ACTION      | A short phrase expressing an action (verb), usually meant to be clicked |
| _WARNING     | Warning when a validation error occurs                                  |
| _TOOLTIP     | Tooltip/toast text that appears on hover                                |
| _USER_HELP   | Long text giving more details about a part of the UI                    |

For a quick reference please see [this cheat sheet](http://www.closurecheatsheet.com/i18n).

#### Capitalization and punctuation
* Only `_USER_HELP` and `_TOOLTIP` messages have standard punctuation and end with `.`
* All of the messages have only their first word capitalized. Exceptions are names which appear in the middle of a message.
* The human translators are supposed to keep the original capitalization and punctuation if applicable to the specific language.

#### Organizing the text variables

Currently, the `MSG` variables are stored in a constant dictionary at the bottom of the controller, which scopes the place (template) where they are used. The variables can then be referenced directly in the respective template's HTML code.

Here is an example of how to organize and use such variables:

```{js}
class exampleController {
  constructor() {
    ...

    /**
     * @export
     */
    this.i18n = i18n;

    ...
  }
}

const i18n = {
  /** @export {string} @desc a simple description for the translator */
  MSG_EXAMPLE_TEXT: goog.getMsg('This is an example'),
  ... // other variables
}
```

```{html}
<html-block>
{::ctrl.i18n.MSG_EXAMPLE_TEXT}}
</html-block>
```

In the HTML code, use one-time bindings like `{{::ctrl.i18n.MSG_EXAMPLE_TEXT}}` for efficiency.

### Localization with `[[Message|Description]]` pattern

Usage of `MSG_` variables in JavaScript and HTML files is not the only way to localize messages. The second option
is to use specific pattern directly in HTML messages:

```
[[This is an example|a simple description for the translator]]
```

As you can see it is the same definition as in above section, but it is a lot easier to use. There is sample usage:

```html
<div>
    [[This is an example|a simple description for the translator]]
</div>
```

#### Warning

This is a quick way of using multiple localized messages without changes in JavaScript files, but it does have few drawbacks. Please keep in mind this method requires some automated work and following rules apply:

 * Message variables are generated automatically during the build process. Their names depend on file name and message position within a file. It means, that if the message will be moved to another file, all its translations will be lost.
 * It is not possible to reference variables from `[[|]]` pattern.

## Translation management

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