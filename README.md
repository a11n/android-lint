# Android Lint API reference guide
The missing documentation on how to write **custom Android Lint rules**.

## Motivation
**Android Lint** was introduced back in December 2011. Since then its IDE integration became better and better. Today Lint is the Android developer’s essential smart companion guaranteeing a good quality of all Android development assets. With the release of the most recent Android Studio version in January 2015 Lint features more than 200 default checks. It checks for potential bugs, bad coding habits, broken conventions and much more.

However, several circumstances exist where it is necessary to extend the default set with custom Lint rules. This counts in particular for large projects with either a huge code base or big, distributed developer teams.

Although Lint is a fundamental part of the Android Developer Tools, the documentation on how to write custom Lint rules is rare and deprecated.

This reference guide aims to fill this gap and provide essential knowledge on how to write **custom Android Lint rules**.

## Structure
The guide is structured into six consecutive sections.

#### 1. [Introduction](1_introduction)
This section introduces into the domain of Android Lint. It briefly describes the purpose, usage and configuration of this tool.

#### 2. [Lint API basics](2_lint_api_basics)
This section covers the basics of the Android Lint API. It explains the core concepts of the API, such as `Issues`, `Detectors`, `Scanners` and `IssueRegistry`.

#### 3. [Getting started](3_getting_started)
After the elaboration of the Lint API theory in [section 1](1_introduction/) and [section 2](2_lint_api_basics/) this chapter provides practical explanations and code to get you started writing your own custom Android Lint rules.

#### 4. [Detectors](4_detectors)
This section demonstrates how to cope with `Detectors`. It showcases simple as well as advanced detecting techniques.

#### 5. [Verification](5_verification)
This section presents how to test and debug custom Lint rules.

#### 6. [Application](6_application)
This section elaborates options to apply your custom Android Lint rules to your Android application project.


## Additional Resources
If you are looking for more information on Lint the following resources can be helpful.

1. [Android Docs on Lint](https://developer.android.com/studio/write/lint.html): Covers how to configure lint
2. [Android's Conceptual Overview of Lint](http://tools.android.com/tips/lint/writing-a-lint-check): Covers the concepts of writing android lint rules.
3. [A Presentation on Lint with Android](https://academy.realm.io/posts/360andev-matthew-compton-linty-fresh-java-android/): Presents is the same information as this guide, in video format.
4. [Writing Lint Rules with Annotations](https://android.jlelse.eu/writing-custom-lint-rules-and-integrating-them-with-android-studio-inspections-or-carefulnow-c54d72f00d30): Walks you through writing a lint rule that enforces an annotation.

## Contributing
Contribution to this guide is appreciated. Please refer to the [contribution guidelines](CONTRIBUTING.md) if you want to contribute something.

## Remark
Google points out very significant, the Lint API **is not final and may change in future releases**.

This reference guide refers to the most recent (June 2015), stable version (24.3.0-beta2) of the API.

## Disclaimer
Please note that this reference guide **does not present any official Android documentation**. It was created by investigating existing specifications and sources.

Also, things described here **may be incorrect**. You are welcome to contribute to this guide in order to correct them.

Please be aware if Google decides to change the API, this guide **may not be up-to-date**. I am maintaining it in my spare time.

## License
&copy;2015 André Diermann

[![License](https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-nc-sa/4.0/)

This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License.
