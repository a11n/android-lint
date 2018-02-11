# 4. Detectors
Detectors represent the actual implementation of a custom Lint rule. Detectors have briefly been described in [section 2.2](../2_lint_basics/#22-detector) already. This section demonstrates in more detail how to cope with `Detectors`. It showcases simple as well as advanced detecting techniques.

**Note:** There exists no official distinction between simple detectors and advanced detectors. It's a classification which has been chosen here in order to emphasize different detecting techniques.

## Simple detectors

>####Definition
> Simple detectors scan isolated artifacts of one type
(e.g. just code or just resources). Scan and evaluation are performed in one phase.

In order to elaborate the aforementioned definition of simple detectors a bit let's have a look at Android's `HardcodedValuesDetecor`, which is a good example of a simple detector. It's responsible for detecting hardcoded values, which means you have defined values in your layouts or menus directly instead of referencing them from a dedicated `strings.xml` file.

In order to detect such flaws only layout or menu files are scanned. In each file all value-containing attributes, such as `text` or `hint`, are visited. Their values are evaluated and in case they are no references, for instance they don't start with `@`, an `Issue` (see section 2.4) is reported. This evaluation is performed during the scan phase directly:

```java
public class HardcodedValuesDetector extends LayoutDetector {

    //omitted for brevity

    @Override
    public boolean appliesTo(@NonNull ResourceFolderType folderType) {
        return folderType == ResourceFolderType.LAYOUT || folderType == ResourceFolderType.MENU;
    }

    @Override
    public void visitAttribute(@NonNull XmlContext context, @NonNull Attr attribute) {
        String value = attribute.getValue();
        if (!value.isEmpty() && (value.charAt(0) != '@' && value.charAt(0) != '?')) {
            // Make sure this is really one of the android: attributes
            if (!ANDROID_URI.equals(attribute.getNamespaceURI())) {
                return;
            }

            context.report(ISSUE, attribute, context.getLocation(attribute),
                String.format("[I18N] Hardcoded string \"%1$s\", should use `@string` resource",
                              value));
        }
    }
}
```

Other examples for simple detector are `SdCardDetector` [2] or `MissingIdDetector` [3].

In general, typical use cases for simple detectors are naming conventions or invalid value detection.

## Advanced detectors

>####Definition
> Advanced detectors scan related artifacts of different types.
Scan and evaluation is performed in two phases.

*This section is still under construction.*

##References
1. https://android.googlesource.com/platform/tools/base/+/android-m-preview/lint/libs/lint-checks/src/main/java/com/android/tools/lint/checks
2. https://android.googlesource.com/platform/tools/base/+/android-m-preview/lint/libs/lint-checks/src/main/java/com/android/tools/lint/checks/SdCardDetector.java
3. https://android.googlesource.com/platform/tools/base/+/android-m-preview/lint/libs/lint-checks/src/main/java/com/android/tools/lint/checks/MissingIdDetector.java

## License
&copy;2015 André Diermann

[![License](https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-nc-sa/4.0/)

This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License.
