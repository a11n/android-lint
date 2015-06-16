# 7. Application
In the preceding sections you have learned how Lint works and how to write custom Android Lint rules. This section elaborates options to apply custom Lint rules to Android application projects.

Basically there are **two approaches to apply custom Lint rules** to your Android application project. Section 7.1 covers the basic approach. It is a simple, straightforward approach. Section 7.2 presents a slightly more complex (in the sense of configuration) and advanced approach, which on the other hand integrates the custom Lint rules closer with the Android application project.

## 7.1 Basic approach
This approach utilizes a basic Lint extension feature. By default, Lint will scan a special folder for custom Lint rules and will consider them during analysis.

In order to apply custom Lint rules to an Android application project using the basic approach two steps are necessary:
1. Assemble your custom Lint rules into a JAR
2. Copy that JAR to `~/.android/lint`

When you run Lint for the next time your custom rules will be applied to your Android application projects.

As you can see, this approach is very simple on the one hand, but on the other hand there are some complications when you think of multi developer teams or CI environments. Its pros and cons are presented in the following listing:

|Pros|Cons|
|----|----|
|just assemble and copy|no straightforward distribution and configuration|
|one resulting JAR|no project-specific rules|
|no changes in the project to analyze|inconvenient for multi developer teams|
|applied for all analyzed projects|inconvenient for CI environments|

## 7.2 Integrated approach
For a long time the basic approach was the only way to apply custom Lint rules to Android application projects. But with the introduction of the AAR format there's now another option. AAR bundles may contain a dedicated `lint.jar` which rules will be considered during Lint analysis. This features a more integrated approach. Now, custom Lint rules can be integrated directly into the Android application project by adding an AAR dependency.

In order to apply custom Lint rules to an Android application project using the integrated approach two steps are necessary:
1. Wrap the custom Lint rules into an AAR container<br/>Example in section 3
2. Make the dedicated Android application project depend on that AAR

  You actually have three options to make your application project depend on an AAR:
  * Copy the AAR to the `libs` folder of your application project and tell Gradle to use this folder for dependencies:

  ```groovy
  dependencies {
    compile fileTree(dir: 'libs', include: '*.jar')
  }
  ```

  This is a convenient option if you have **company-wide- or team-wide-conventions** you want Lint to enforce. You will most probably have a dedicated Lint rule project and bundle the resulting AAR of that project with each Android application project.

  * Deploy your AAR to a central- or company-repository and tell Gradle to use this as dependency. Please note the `@aar` specifier:

  ```groovy
  dependencies {
    compile 'your.package.name:custom-lint:1.0.0@aar'
  }
  ```

  This is a convenient option if you have **company-wide- or team-wide-conventions** you want Lint to enforce. You will most probably have a dedicated Lint rule project and bundle the resulting AAR of that project with each Android application project.

  * Have a **Java module for the Lint rules** and an **Android library module as wrapper** in your application project:

  ```
  Android application project
  --app         #default Android application module
  --lint        #Android library, acts as wrapper for the Lint rules
  --lintrules   #Java module with your custom Lint rules
  ```

  The trick is done by the following Groovy code in your wrapper's build.gradle. It hooks into Android's build lifecycle and injects a `lint.jar` with custom rules before the AAR is bundled:

  ```groovy
  project.afterEvaluate {
    def compileLint = project.tasks.getByPath(':lint:compileLint')
    compileLint.dependsOn ':lintrules:jar'
    compileLint << {
      copy{
        from '../lintrules/build/libs'
        into 'build/intermediates/lint'
      }
    }
  }
  ```

  This is a convenient option if you have **project-specific conventions**. If you need to add or modify any project-specific convention you do not need to re-build and re-deploy a separate AAR.

This approach is a good choice for larger projects with multiple developers working on it. It is furthermore also handy for CI environments since the custom Lint rules already come bundled with the project to build. Please find an overview of the pros and cons of this approach in the following listing:

|Pros|Cons|
|----|----|
|allows project specific rules|changes in project required|
|integrated within project|not applied for all analyzed projects|
|ideal for multi developer teams|*(no official documentation on how to wrap into AAR bundle available)*|
|perfect for CI environments||

## License
&copy;2015 André Diermann

[![License](https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-nc-sa/4.0/)

This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License.
