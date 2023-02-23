A **Locale category** is a collection of closely-related settings that control the way information is presented to the user depending on regional and/or cultural characteristics. Examples of locale categories are:

- `LANG`: Language
- `LC_CTYPE`: Character classification and case conversion settings
- `LC_COLLATE`: String collation (ordering) settings
- `LC_NUMERIC`: Number format settings
- `LC_TIME`: Date-time format settings

A **locale** is a specific implementation of one or more locale categories.

A **locale identifier** is a string with the format `language[_territory][.codeset][@modifier]` that identifies a locale. Examples are `en_US` or `de_DE.utf-8@euro`.

A locale-aware system provides:

- A collection of supported locales. In Linux this is done by downloading and installing them, using either the package manager or some other tool.
- A way for the user to specify the desired locale to be used for each of the supported locale categories. In Linux this is done with environment variables. 
- A way for programs to process and present information according to those user-specified locales. In Linux this is done by... (how is this really done in Linux? there seems to be some locale-related functions provided by glibc like `nl_langinfo`... is this the whole story?).

### Locales in Debian

In Debian the following utilities are used to control locale-related configuration:

- `locale` and `localedef` from the package `libc-bin` (from the `glibc` source package)
- `update-locale` and `locale-gen` from the package `locales` (from the `glibc` source package).

In Debian new locales are installed using `locale-gen`.

In CentOS new locales are installed by installing the corresponding `langpacks-*` package.