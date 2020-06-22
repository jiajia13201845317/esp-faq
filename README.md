# ESP-FAQ Framework

* [中文版](./README_CN.md)

ESP-FAQ is a summary document for common problems launched by Espressif.

## ESP-FAQ Document framework

### language support

* The document framework supports reStructuredText and Markdown lightweight markup languages.

### Document structure

``` bash
docs
├── _static
│   ├── espressif-logo.svg       # Web log
│   └── theme_overrides.css      # Web Style Sheet
├── zh_CN
│   ├── _static                  # Inline resources
│   ├── get-started              # Get started
│   ├── development-environment  # Development environment
│   ├── application-solution     # Application solution
│   ├── hardware-related         # Hardware related
│   ├── software-framework       # Software framework
│   ├── test-verification        # Test verification
│   ├── config.py                # Sphinx config
│   ├── Makefile                 # Compile file
│   └── index.rst                # Home
└── en
    ├── _static                  # Inline resources
    ├── get-started              # Get started
    ├── development-environment  # Development environment
    ├── application-solution     # Application solution
    ├── hardware-related         # Hardware related
    ├── software-framework       # Software framework
    ├── test-verification        # Test verification
    ├── config.py                # Sphinx config
    ├── Makefile                 # Compile file
    └── index.rst                # Home
```

### Document Contribution

* Please read the code [Document Contribution](docs/zh_CN/get-started/document-contribution.md)

## Resources

* Documentation for the latest version: https://docs.espressif.com/projects/esp-faq/. This documentation is built from the [docs directory](docs) of this repository.

* The [esp32.com forum](https://esp32.com/) is a place to ask questions and find community resources.

* [Check the Issues section on github](https://github.com/espressif/esp-faq/issues) if you find a bug or have a feature request. Please check existing Issues before opening a new one.

* If you're interested in contributing to ESP-IDF, please check the [Contributions Guide](docs/zh_CN/get-started/document-contribution.md).
