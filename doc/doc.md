<img align="left" width="70" height="70" src="https://github.com/user-attachments/assets/95ed169c-4c78-455a-98ac-81252d5c0195" />

# Testowl

Testowl is a continuous test runner that runs only unit tests that are impacted by current code changes. 

**This is an early access version. Please note that the plugin is not yet in a stable state and bugs are to be expected.
The plugin will be available for a fee at a later date(as a "paid plugin").
Until then, the early access version of the plugin can be used free of charge under the conditions of the [LICENSE](/License.txt).**


## Getting Started

### Install the Plugin from the Jetbrains Marketplace

1) To be able to download and install Testowl from the Jetbrains Marketplace you first need to the eap release channel of Testowl to your repsitories.
Go to **Settings -> Plugins**. Click the settings icon at the top of the window and choose *"Manage plugin repositories"*.

  ![manageRepositories](https://github.com/user-attachments/assets/f75dc329-aaf0-4038-b3ca-08a22598babd)


   Add the following repository:

   ```
   https://plugins.jetbrains.com/plugins/eap/io.testowl.testowl-plugin
   ```
   ![452551860-903ff5b7-03d1-4803-bdbc-55fc1c5c815a](https://github.com/user-attachments/assets/9cd883f9-3fb9-4fb4-b7d0-ad1ca08bc045)


2) Now search for Testowl and click install.

3) Continue with this tutorial at "Start working with Testowl after installation".


### Start working with Testowl after installation

1) After installation, you will find the Testowl icon on the right-hand side of the IDE.
   Click on the icon to open the tool window. At this point, no tests may be displayed in the tool window.
   
   ![447566487-dc270c4e-d0c1-488d-bdc7-5b23d523f5ed](https://github.com/user-attachments/assets/19ad3d1c-416d-4502-9373-212207a9a356)


3) The plugin activates the autobuild feature of the IDE. This is required to run tests automatically. Check in the Autobuild Tool window whether the project has been built successfully. In the OK case, the following is displayed: “No compilation problems found”

   ![autobuild](https://github.com/user-attachments/assets/e68f286f-ba73-4b69-94d5-ed2dc7611c92)

    In some cases to make the autbuild work initially a rebuild of the project is required. If you see error messages in the Autobuild window, carry out a          rebuild (**Build -> Rebuild Project**).

   ![autobuild_error](https://github.com/user-attachments/assets/1407b6b4-7dc0-40ff-9ecc-4c79c34b8ee8)




5) If the Autbuild is working all classes will be compiled by the autobuild. The Testowl plugin then analyzes classes and source files and searches for test classes in the project. This can take a while for larger projects. The project status bar (bottom right in the IDE) displays “Initializing continuous test runner” or alternatively “Running tests”.

6) Once initialization is complete, all tests of the project are initially executed. For changes that happen afterwards, only a subset (only relevant tests) is executed.

>Especially with smaller projects, these steps happen so quickly that you as the user don't even notice them. In the best case scenario, you open the tool window and the initial test run is already complete.

>If the tests are not executed after initialization, you can do the following:  
> 1) Check the autobuild. Are problems reported here? If so, fix them. If necessary, a project rebuild can help.
> 2) Are the test directories in the IDE marked as test directories? 
> 3) Are the source directories marked as source directories?
> 4) You can try to initialize the Testowl plugin again. Use the reset button at the top left of the tool window.

5) In some projects, additional configuration of the plugin is necessary. For example, the tests may need to be executed with additional JVM arguments or a special working directory may be required. In such cases, the configuration of the plugin must be adjusted and then a reinitialization (button in the top left of the tool window) must be initiated.
For details refer to [Plugin Configuration](#Plugin-Configuration).

6) Performance tuning: Setting idle save period

If autobuild is enabled, IntelliJ IDEA compiles the sources whenever source files are saved, whereupon Testowl analyzes the classes and executes necessary tests. To make the process more performant, the time until the source files are saved can be shortened. 

To do this, open **Settings -> System Settings** and enter 1 second under **Autosave -> “Save files if the IDE is idle for“**.

<img width="425" alt="perfromance_tuning1" src="https://github.com/user-attachments/assets/af2ede30-4089-49b4-b57b-aba2f2b4db50" />


The idle time can be shortened even further by reducing the value **"compiler.document.save.trigger.delay ”** in the IntelliJ IDEA registry. However, the value should not be configured below 500ms.  The registry can be opened with the Search everywhere menu (2x Shift) and search for “Registry”. 

<img width="425" alt="performance_tuning2" src="https://github.com/user-attachments/assets/b46b4660-c117-4d5f-ae1b-790dbb13b970" />


**Congratulations you successfully installed and setup the Testowl plugin!**   


## User Interface 

### Coverage Markers

A line that is covered by any test in your project is marked with a green (only successful tests) or a red (at least one faulty test). This enables focused work on the code without having to perform a context switch, as the most relevant information (tests OK or tests not OK) is always present in the editor. 

![coverage_markers](https://github.com/user-attachments/assets/be336e21-9ebf-4564-9044-007f5d47bf11)


### Show covering Tests

Clicking on a coverage marker opens a list of all test cases that cover this line. This not only provides detailed insights into the code coverage, but also enables quick navigation between production and test code.

![show_covering_tests](https://github.com/user-attachments/assets/904a6a93-897d-40af-a690-0c306eeff388)


### Tool Window

While the coverage markers provide an overview of the test status of the code currently being processed, the Window tool provides an overview of the test status of the entire project and allow to inspect failing tests further by viewing the standard output or the stack trace.

#### Reset Button

This button enables a complete reset of the plugin for this project. This means that all classes are reanalyzed and all tests are executed again. For larger projects, a reset can take a while.

![tool_window](https://github.com/user-attachments/assets/88922ec2-a04f-46b5-85ad-08639d8d519b)


## Plugin Configuration
The configuration can be used to change the working directory, define the log level, specify additional JVM parameters, or set the timeout of tests. To customize the plugin configuration, a file **“testowl.properties”** must be created in the root directory of the project. If required, the file can also be committed (and pushed) so that other team members can use the same configuration. 

> The plugin configuration is always reloaded when the IDE is restarted or reset. If a plugin configuration is created or changed, either the IDE must be restarted or a reset must be performed (see [Reset button](#Reset-button)).

### Working directory

By default, the working directory of the module specified by IntellIJ is used. This means that in most cases the root directory of the module is used. Depending on the user configuration in the IDE, however, this can be different.

If you want to use a different working directory, add the following entry to the configuration:

```
<moduleName>.configuration.testWorkingDirectory = <Path>
``` 

In this case, the working directory is set for a specific module. If the working directory is the same for all modules in the project, the working directory can also be set for the entire project. In this case, the entry looks like this:

```
configuration.testWorkingDirectory = <Path>
```

> The path can be specified relatively or absolutely. If a relative path is specified, the assumed root directory is the root directory of the project.


### Log Level

Due to the early development status of the Testowl plugin, the default logging level is currently still set to **debug**. If for some reason a different log level should be set, this is possible via the plugin configuration. 
The following entry can be set in the configuration file for this purpose: 

```
configuration.logLevel = debug | info | warn | error
```

To set the log level to “info”, for example, the following entry is required:

```
configuration.logLevel = info
```

> The log level can only be set project-wide.

### Additional Jvm Arguments

To start new tests, the JVM is started roughly as follows: 

```
<jvm> -ea -cp <classpath> TestowlTestRunner
```

The JVM and the classpath depend on the module and are configured by the user in IntelliJ IDEA.  The parameter **-ea(enable assertions)** is necessary so that assertions are not ignored in the test environment. 

If the JVM is to be started with additional arguments, these can be specified in the plugin configuration either on a module-specific or project-wide basis. 

To add JVM parameters for a specific module, the following entry can be added to the plugin configuration:

```
<moduleName>.configuration.additionalJvmArgs=jvmArg1 jvmArg2 jvmArg3 ...
```

To add JVM parameters project-wide, the following entry can be added: 

```
configuration.additionalJvmArgs=jvmArg1 jvmArg2 jvmArg3 ...
```

> Please note that a module-specific configuration overwrites the project-specific configuration for the specified module.


#### Example Language, country and time zone should be set should be set for the myTest module

In this case, the following entry should be added to the plugin configuration:

```
myTest.configuration.additionalJvmArgs = -Duser.language="en" -Duser.country="US" -Duser.timezone="America/Los_Angeles"
```

### Test Timeout

By default, test cases are executed with a timeout of one minute. This means that if a single test case is not completed within one minute, the test is marked as faulty and the next test is executed. 

To shorten or increase the timeout, the timeout can be set project-wide in the configuration with the following entry: 

```
configuration.testTimeout = <timeout in milliseconds>
```

If the timeout is to be set to a value of one second, for example, the following entry would have to be added to the configuration: 

```
configuration.testTimeout = 1000
```

> The test timeout can only be set project-wide.

## Limitations

### Parallelization of Test Runs

Currently parallization is not supported, although it will be supported in the future. This means that if you have configured parallel test execution in Junit 5, for example, Testowl's intelligent test selection will not work reliably. Under certain circumstances, not all impacted tests will be executed.

### Build Frameworks

Tests are executed with the Testowl test runner itself. No build framework such as Gradle, Maven or Ant is used for this. This means that tasks upstream of testing that are executed in the build framework are not executed in Testowl's test runner. Please make sure that all tests can be executed independently without being dependent on the build system. Various properties such as JVM parameters can be specified via the plugin configuration (see [Plugin Configuration](#Plugin-Configuration)).

## Further Resources

[Testowl product video](https://youtu.be/_bDQ2y3zFMs)

## Open Source Libraries and Licenses


| Library                      | Version      | License                     | Notes                      |Website                      |
| :--------------------------- | :----------- | :-------------------------- | :------------------------- | :-------------------------  |
| junit : junit                 | 4.13.2       | [Eclipse Public License - v 1.0](/doc/licenses/LICENSE-junit.txt)  || https://github.com/junit-team/junit4 |
| org.junit.jupiter : junit-jupiter-api            | 5.11.4       | [Eclipse Public License - v 2.0](/doc/licenses/LICENSE-junit5.md)  || https://junit.org/junit5/ |
| org.junit.jupiter : junit-jupiter-engine         | 5.11.4       | [Eclipse Public License - v 2.0](/doc/licenses/LICENSE-junit5.md)  || https://junit.org/junit5/ |
| org.junit.vintage : junit-vintage-engine         | 5.11.4       | [Eclipse Public License - v 2.0](/doc/licenses/LICENSE-junit5.md)  || https://junit.org/junit5/ |
| org.junit.jupiter : junit-jupiter-params         | 5.11.4       | [Eclipse Public License - v 2.0](/doc/licenses/LICENSE-junit5.md)  || https://junit.org/junit5/ |
| org.junit.platform : junit-platform-launcher      | 1.11.4       | [Eclipse Public License - v 2.0](/doc/licenses/LICENSE-junit5.md)  || https://junit.org/junit5/ |
| org.junit.platform : junit-platform-engine        | 1.11.4       | [Eclipse Public License - v 2.0](/doc/licenses/LICENSE-junit5.md)  || https://junit.org/junit5/ |
| org.ow2.asm : asm                          | 9.7.1        | [BSD 3-clause](/doc/licenses/LICENSE-asm.txt)  || https://asm.ow2.io |
| org.ow2.asm : asm-commons                  | 9.7.1        | [BSD 3-clause](/doc/licenses/LICENSE-asm.txt)  || https://asm.ow2.io |
| org.ow2.asm : asm-util                     | 9.7.1        | [BSD 3-clause](/doc/licenses/LICENSE-asm.txt)  || https://asm.ow2.io |
| org.ow2.asm : asm-tree                     | 9.7.1        | [BSD 3-clause](/doc/licenses/LICENSE-asm.txt)  || https://asm.ow2.io |
| org.ow2.asm : asm-analysis                     | 9.7.1        | [BSD 3-clause](/doc/licenses/LICENSE-asm.txt)  || https://asm.ow2.io |
| org.apache.commons : commons-lang3                | 3.17.0       | [Apache License 2.0](/doc/licenses/ApacheLicense2.0.txt)  |1. NOTICE txt file according to section 4d: [NOTICE.txt](/doc/licenses/commons-lang/NOTICE.txt) <br> 2. Information according to section 4b: Packages and classfiles of the library were renamed for technical reasons. This is also known as "shading" and was done via the [Maven Shade plugin](https://maven.apache.org/plugins/maven-shade-plugin/).| https://github.com/apache/commons-lang |
| commons-io : commons-io                   | 2.18.0       | [Apache License 2.0](/doc/licenses/ApacheLicense2.0.txt)  |1. NOTICE txt file according to section 4d: [NOTICE.txt](/doc/licenses/commons-io/NOTICE.txt) <br> 2. Information according to section 4b: Packages and classfiles of the library were renamed for technical reasons. This is also known as "shading" and was done via the [Maven Shade plugin](https://maven.apache.org/plugins/maven-shade-plugin/).| https://github.com/apache/commons-io |
| org.apache.commons : commons-text                 | 1.13.0       | [Apache License 2.0](/doc/licenses/ApacheLicense2.0.txt)  |1. NOTICE txt file according to section 4d: [NOTICE.txt](/doc/licenses/commons-text/NOTICE.txt) <br> 2. Information according to section 4b: Packages and classfiles of the library were renamed for technical reasons. This is also known as "shading" and was done via the [Maven Shade plugin](https://maven.apache.org/plugins/maven-shade-plugin/).| https://github.com/apache/commons-text |
| commons-codec:commons-codec                | 1.17.1       | [Apache License 2.0](/doc/licenses/ApacheLicense2.0.txt)  |1. NOTICE txt file according to section 4d: [NOTICE.txt](/doc/licenses/commons-codec/NOTICE.txt) <br> 2. Information according to section 4b: Packages and classfiles of the library were renamed for technical reasons. This is also known as "shading" and was done via the [Maven Shade plugin](https://maven.apache.org/plugins/maven-shade-plugin/).| https://github.com/apache/commons-codec |
| om.google.code.gson : gson                         | 2.11.0       | [Apache License 2.0](/doc/licenses/ApacheLicense2.0.txt)  |Information according to section 4b: Packages and classfiles of the library were renamed for technical reasons. This is also known as "shading" and was done via the [Maven Shade plugin](https://maven.apache.org/plugins/maven-shade-plugin/).| https://github.com/google/gson |
| io.github.java-diff-utils : java-diff-utils              | 4.15         | [Apache License 2.0](/doc/licenses/ApacheLicense2.0.txt)  |Information according to section 4b: Packages and classfiles of the library were renamed for technical reasons. This is also known as "shading" and was done via the [Maven Shade plugin](https://maven.apache.org/plugins/maven-shade-plugin/).| https://github.com/java-diff-utils/java-diff-utils |
| org.javassist : javassist                    | 3.30.2-GA          | [Apache License 2.0](/doc/licenses/ApacheLicense2.0.txt)  |Information according to section 4b: Packages and classfiles of the library were renamed for technical reasons. This is also known as "shading" and was done via the [Maven Shade plugin](https://maven.apache.org/plugins/maven-shade-plugin/).| https://www.javassist.org |
| org.tinylog : tinylog-api                  | 2.7.0                | [Apache License 2.0](/doc/licenses/ApacheLicense2.0.txt)  |Information according to section 4b: Packages and classfiles of the library were renamed for technical reasons. This is also known as "shading" and was done via the [Maven Shade plugin](https://maven.apache.org/plugins/maven-shade-plugin/).| https://tinylog.org |
| org.tinylog : tinylog-impl                 | 2.7.0                | [Apache License 2.0](/doc/licenses/ApacheLicense2.0.txt)  |Information according to section 4b: Packages and classfiles of the library were renamed for technical reasons. This is also known as "shading" and was done via the [Maven Shade plugin](https://maven.apache.org/plugins/maven-shade-plugin/).| https://tinylog.org |
| apiguardian : api                | 1.1.2                | [Apache License 2.0](/doc/licenses/ApacheLicense2.0.txt)  || https://github.com/apiguardian-team/apiguardian |
| hamcrest : core                | 1.3               | [BSD](/doc/licenses/LICENSE-hamcrest.txt)  || https://hamcrest.org/JavaHamcrest/ |
| opentest4j               | 1.3.0               | [Apache License 2.0](/doc/licenses/ApacheLicense2.0.txt)  || https://github.com/ota4j-team/opentest4j  |
| com.google.errorprone : error_prone_annotations              | 2.27.0              | [Apache License 2.0](/doc/licenses/ApacheLicense2.0.txt)  |Information according to section 4b: Packages and classfiles of the library were renamed for technical reasons. This is also known as "shading" and was done via the [Maven Shade plugin](https://maven.apache.org/plugins/maven-shade-plugin/).| https://github.com/ota4j-team/opentest4j  |

