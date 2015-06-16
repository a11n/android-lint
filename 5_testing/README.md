# 6. Testing

**!!! Due to recent releases: This section is still under construction. !!!**

Testing is an important activity in software development. This counts in particular for custom Lint rules, since they validate your code for convention flaws and other defects.

For a long time there exists no convenient way to test custom Lint rules. But fortunately end of May 2015 Google released an [official library](https://bintray.com/android/android-tools/com.android.tools.lint.lint-tests/view) for testing custom Lint rules.

The following example demonstrate the basic test principles [1]. More Examples on how to test custom Lint rules can be found in [Android's Lint test source code](https://android.googlesource.com/platform/tools/base/+/master/lint/cli/src/test/java/com/android/tools/lint/checks/):

```java
public class HardcodedValuesDetectorTest  extends AbstractCheckTest {
    @Override
    protected Detector getDetector() {
        return new HardcodedValuesDetector();
    }
    public void testStrings() throws Exception {
        assertEquals(
            "res/layout/accessibility.xml:3: Warning: [I18N] Hardcoded string \"Button\", should use @string resource [HardcodedText]\n" +
            "    <Button android:text=\"Button\" android:id=\"@+id/button1\" android:layout_width=\"wrap_content\" android:layout_height=\"wrap_content\"></Button>\n" +
            "            ~~~~~~~~~~~~~~~~~~~~~\n" +
            "res/layout/accessibility.xml:6: Warning: [I18N] Hardcoded string \"Button\", should use @string resource [HardcodedText]\n" +
            "    <Button android:text=\"Button\" android:id=\"@+id/button2\" android:layout_width=\"wrap_content\" android:layout_height=\"wrap_content\"></Button>\n" +
            "            ~~~~~~~~~~~~~~~~~~~~~\n" +
            "0 errors, 2 warnings\n",
            lintFiles("res/layout/accessibility.xml"));
    }
}
```

1. A test class needs to extend `LintDetectorTest` (Note: In the example above another class is extended. This is an Android internal class, which in fact also extends `LintDetectorTest`).
2. `getDetector()` needs to be overwritten and to return the `Detector` which should be tested.
3. An assertions is performed on the result of `lintFiles()` which takes a list of relative file names to test as argument.

## References

1. https://android.googlesource.com/platform/tools/base/+/master/lint/cli/src/test/java/com/android/tools/lint/checks/HardcodedValuesDetectorTest.java

## License
&copy;2015 André Diermann

[![License](https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-nc-sa/4.0/)

This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License.
