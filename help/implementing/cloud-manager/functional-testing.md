---
title: Functional Testing - Cloud Services
description: Functional Testing - Cloud Services
exl-id: 7eb50225-e638-4c05-a755-4647a00d8357
---
# Functional Testing {#functional-testing}


>[!CONTEXTUALHELP]
>id="aemcloud_nonbpa_functionaltesting"
>title="Functional Testing"
>abstract="Functional testing is categorized into three types: Product Functional Testing, Custom Functional Testing and Custom UI Testing"

Functional testing is categorized into three types:


* [Product Functional Testing](#product-functional-testing)
* [Custom Functional Testing](#custom-functional-testing)
* [Custom UI Testing](/help/implementing/cloud-manager/ui-testing.md#custom-ui-testing)

## Product Functional Testing {#product-functional-testing}

Product Functional Tests are a set of stable HTTP integration tests (ITs) around core functionality in AEM (for example, authoring and replication) that prevent customer changes to their application code from being deployed if it breaks this core functionality.

Product Functional Tests run automatically whenever a customer deploys new code to Cloud Manager and cannot be skipped.

Refer to [Product Functional tests](https://github.com/adobe/aem-test-samples/tree/aem-cloud/smoke) for sample tests.

## Custom Functional Testing {#custom-functional-testing}

The Custom Functional testing step in the pipeline is always present and cannot be skipped. 

The build should produce either zero or one test JARs. If it produces zero test JARs, the test step passes by default. If the build produces more than one test JARs, which JAR is selected is non-deterministic.

>[!NOTE]
>The **Download Log** button allows access to a ZIP file containing the logs for the test execution detailed form. These logs do not include the logs of the actual AEM runtime process – those can be accessed using the regular Download or Tail Logs functionality. Refer to [Accessing and Managing Logs](/help/implementing/cloud-manager/manage-logs.md) for more details.


## Writing Functional Tests {#writing-functional-tests}

Customer-written functional tests must be packaged as a separate JAR file produced by the same Maven build as the artifacts to be deployed to AEM. Generally this would be a separate Maven module. The resulting JAR file must contain all required dependencies and would generally be created using the maven-assembly-plugin using the jar-with-dependencies descriptor. 

In addition, the JAR must have the Cloud-Manager-TestType manifest header set to integration-test. In the future, it is expected that additional header values will be supported. An example configuration for the maven-assembly-plugin is:

```java
<build>
    <plugins>
        <!-- Create self-contained jar with dependencies -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-assembly-plugin</artifactId>
            <version>3.1.0</version>
            <configuration>
                <descriptorRefs>
                    <descriptorRef>jar-with-dependencies</descriptorRef>
                </descriptorRefs>
                <archive>
                    <manifestEntries>
                        <Cloud-Manager-TestType>integration-test</Cloud-Manager-TestType>
                    </manifestEntries>
                </archive>
            </configuration>
            <executions>
                <execution>
                    <id>make-assembly</id>
                    <phase>package</phase>
                    <goals>
                        <goal>single</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
```

Within this JAR file, the class names of the actual tests to be executed must end in `IT`. 

For example, a class named `com.myco.tests.aem.it.ExampleIT` would be executed, but a class named `com.myco.tests.aem.it.ExampleTest` would not.
 
Furthermore, to exclude test code from the coverage check of the code scanning, the test code must be below a package named `it` (the coverage exclusion filter is `**/it/**/*.java`).

The test classes need to be normal JUnit tests. The test infrastructure is designed and configured to be compatible with the conventions used by the aem-testing-clients test library. Developers are strongly encouraged to use this library and follow its best practices. Refer to [Git Link](https://github.com/adobe/aem-testing-clients) for more details.

## Local Test Execution {#local-test-execution}

As the test classes are JUnit tests, they can be run from mainstream Java IDEs like Eclipse, IntelliJ, NetBeans, and so on. 

However, when running these tests, it will be necessary to set a variety of system properties expected by the aem-testing-clients (and the underlying Sling Testing Clients). 

The system properties are as follows:

* `sling.it.instances - should be set to 2`
* `sling.it.instance.url.1 - should be set to the author URL, for example, http://localhost:4502`
* `sling.it.instance.runmode.1 - should be set to author`
* `sling.it.instance.adminUser.1 - should be set to the author admin user, e.g. admin`
* `sling.it.instance.adminPassword.1 - should be set to the author admin password`
* `sling.it.instance.url.2 - should be set to the author URL, for example, http://localhost:4503`
* `sling.it.instance.runmode.2 - should be set to publish`
* `sling.it.instance.adminUser.2 - should be set to the publish admin user, for example, admin`
* `sling.it.instance.adminPassword.2 - should be set to the publish admin password`
