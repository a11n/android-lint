# 2. Lint API basics

This section covers the core concepts of the Android Lint API. The subsequent sections contain detailed information on the most important parts of the API.

## 2.1 IssueRegistry

>####Definition
>An `IssueRegistry` is a registry which provides a list of checks to be performed on an Android project.

All checks* which should be considered during Lint analysis need to be registered in an `IssueRegistry`. The built-in checks are registered within the `BuiltinIssueRegistry` class in the `com.android.tools.lint.checks` package.

In order to make custom Lint checks available for Lint analysis you need to subclass `IssueRegistry` and override its `getIssues` method which provides a `List` of `Issues`. Here is a straightforward example:

```java
public class CustomIssueRegistry extends IssueRegistry {
  @Override
  public List<Issue> getIssues() {
    return Arrays.asList(             //Note:
      MyCustomCheck.ISSUE,            //A check is actually a detector.
      MyAdvancedCheck.AN_ISSUE,       //One detector can report
      MyAdvancedCheck.ANOTHER_ISSUE   //multiple types of issues.
    );
  }
}
```

Last but not least, it is necessary to reference your `IssueRegistry` class within your `MANIFEST`, such that it can be found by Lint. For Gradle-based projects this could be achieved by:

```groovy
jar {
    manifest {
        attributes 'Lint-Registry': 'your.package.name.CustomIssueRegistry'
    }
}
```

You will find a complete example with a lot more code in section 3.

\* Note that you actually register `Issues` rather than checks. An explanation of `Issues` will be provided in section 2.4.

## 2.2 Detector

>####Definition
>A detector is responsible for scanning through code and finding issue instances and reporting them.

The `Detector` class is the central element for the definition of Lint rules. More precisely a `Detector` is an implementation of a Lint rule. It contains the logic to find certain problems in your development artifacts and map it to particular `Issues`. You will learn more about `Issues` in section 2.4.

In order to write a custom Lint rule you will need to subclass `Detector`. For certain use cases the Lint API also provides specific `Detectors` for your convenience, such as `LayoutDetector` or `ResourceXmlDetector` - but they are in turn also sub-classes of `Detector`.

Besides extending the `Detector` class you also need to implement a `Scanner` interface which defines how your `Detector` will scan the development artifacts! **By default the `Detector` class declares all methods of all available `Scanners` but you will notice that only the ones of the implemented interface(s) will get called!**

The different types of `Scanners` will be presented in the next section.

## 2.3 Scanner

>####Definition
>A scanner is a specialized interface for detectors.

Currently seven different types of `Scanner` exist:
* **JavaScanner:** Scans Java source file parse trees
* **ClassScanner:** Scans Java class files
* **BinaryResourceScanner:** Scans binary resource files
* **ResourceFolderScanner:** Scans resource folders (the folder directory itself, not the individual files within it).
* **XmlScanner:** Scans XML files.
* **GradleScanner:** Scans Gradle files.
* **OtherFileScanner:** Scans other files.

As already mentioned before, you want to implement one or more `Scanner` interfaces in order to let your custom `Detector` attend certain kind of development artifacts. Here is a short example which outlines how a certain `Scanner` assists you to scan for particular aspects of different development artifacts:

|JavaScanner|XmlScanner|
|-----------|----------|
|`applicableSuperClasses()`|`getApplicableElements()`|
|`checkClass(...)`|`visitElement(...)`|
|`getApplicableMethodNames()`|`getApplicableAttributes()`|
|`visitMethod(...)`|`visitAttribute(...)`|
|...|...|

As you can see, the `JavaScanner` interface provides dedicated methods for scanning Java files whereas the `XmlScanner` offers methods dedicated for XML files.

**Note:** There is a relation between `Scanners` and the declared `Scope`. `Scope` is described in the next section.

## 2.4 Issue

>####Definition
>An issue is a type of problem you want to find and show to the user. An issue has an associated description, fuller explanation, category, priority, etc.

So an `Issue` is the link between all previously described concepts. An `Issue` is registered within an `IssueRegistry`, such that Lint is aware of it, and it is detected and reported by a `Detector`, so that it becomes visible to the user. A `Detector` can report more than one type of `Issue`.

Furthermore, an `Issue` is just data. It's a single, final class. In contrast to the other classes it's not subclassed, it's created by a static factory method:

```java
public static final Issue ISSUE = Issue.create(
    "HelloWorld",                                        //ID
    "Unexpected application title",                      //brief description
    "The application title should state 'Hello world'",  //explanation
    Category.CORRECTNESS,                                //category
    5,                                                   //priority
    Severity.INFORMATIONAL,                              //severity
    new Implementation(                                  //implementation
        HelloWorldDetector.class,                        //detector
        Scope.MANIFEST_SCOPE                             //scope
    )
);
```

As mentioned in section 2.1 already you do not register checks within the `IssueRegistry` directly. You rather register `Issues`. As you can see from the previous example the link to the `Detector` is done by an `Implementation`. The `Implementation` also defines a `Scope`, which represents the set of files a detector must consider when performing its analysis. `Scope` in fact is an enumeration which contains all kind of relevant `Scopes`, such as `MANIFEST_SCOPE` or `JAVA_FILE_SCOPE`. Last but not least you see that a `Severity` is assigned to the `Issue`. `Severity` is also an enumeration with values such as `ERROR` or `INFORMATIONAL`. Note that certain `Severities` might let your build fail.

## License
&copy;2015 André Diermann

[![License](https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-nc-sa/4.0/)

This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License.
