# Using the Java platform<a name="service-source-code-java"></a>

The AWS App Runner Java platform provides managed runtimes\. Each runtime makes it easy to build and run containers with web applications based on a Java version\. When you use a Java runtime, App Runner starts with a managed Java runtime image\. This image is based on the [Amazon Linux Docker image](https://hub.docker.com/_/amazonlinux) and contains the runtime package for a version of Java and some tools\. App Runner uses this managed runtime image as a base image, and adds your application code to build a Docker image\. It then deploys this image to run your web service in a container\.

 You specify a runtime for your App Runner service when you [create a service](manage-create.md) using the App Runner console or the [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) API\. You can also specify a runtime as part of your source code\. Use the `runtime` keyword in a [App Runner configuration file](config-file.md) that you include in your code repository\. The naming convention of a managed runtime is *<language\-name><major\-version>*\. 

At this time, all the supported Java runtimes are based on Amazon Corretto\. For valid Java runtime names, see [Java runtime release information](service-source-code-java-releases.md)\.

App Runner updates the runtime for your service to the latest version on every deployment or service update\. If your application requires a specific version of a managed runtime, you can specify it using the `runtime-version` keyword in the [App Runner configuration file](config-file.md)\. You can lock to any level of version \(for example, major or minor\), and App Runner only makes lower\-level updates to the runtime of your service\.

Version syntax for Amazon Corretto runtimes:


| **Runtime** | **Syntax** | **Example** | 
| --- | --- | --- | 
|  corretto11  |  `11.0[.openjdk-update[.openjdk-build[.corretto-specific-revision]]]`  |  `11.0.13.08.1`  | 
|  corretto8  |  `8[.openjdk-update[.openjdk-build[.corretto-specific-revision]]]`  |  `8.312.07.1`  | 

The following examples demonstrate version locking:
+ `11.0.13` – Lock the Open JDK update version\. App Runner updates only Open JDK and Amazon Corretto lower\-level builds\.
+ `11.0.13.08.1` – Lock to a specific version\. App Runner doesn't update your runtime version\.

**Topics**
+ [Java runtime configuration](#service-source-code-java.config)
+ [Java runtime examples](#service-source-code-java.examples)
+ [Java runtime release information](service-source-code-java-releases.md)

## Java runtime configuration<a name="service-source-code-java.config"></a>

When you choose a managed runtime, you must also configure, as a minimum, build and run commands\. You configure them while [creating](manage-create.md) or [updating](manage-configure.md) your App Runner service\. There are a few ways to do it:
+ **Using the App Runner console** – Specify the commands in the **Configure build** section of the creation process or configuration tab\.
+ **Using the App Runner API** – Call [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) or [UpdateService](https://docs.aws.amazon.com/apprunner/latest/api/API_UpdateService.html)\. Specify the commands using the `BuildCommand` and `StartCommand` members of the [CodeConfigurationValues](https://docs.aws.amazon.com/apprunner/latest/api/API_CodeConfigurationValues.html) data type\.
+ **Using a [configuration file](config-file.md)** – Specify one or more build commands in up to three build phases, and a single run command that serves to start your application\. There are additional optional configuration settings\.

Providing a configuration file is optional\. When creating an App Runner service using the console or the API, you specify if App Runner gets your configuration settings directly during creation or from a configuration file\.

## Java runtime examples<a name="service-source-code-java.examples"></a>

The following examples show App Runner configuration files for building and running a Java service\. The last example is the source code for a complete Java application that you can deploy to a Corretto 11 runtime service\.

### Minimal Corretto 11 configuration file<a name="service-source-code-java.examples.minimal"></a>

This example shows a minimal configuration file that you can use with a Corretto 11 managed runtime\. For the assumptions that App Runner makes with a minimal configuration file, see [Configuration file examples](config-file-examples.md#config-file-examples.managed)\.

**Example apprunner\.yaml**  

```
version: 1.0
runtime: corretto11
build:
  commands:    
    build:
      - mvn clean package
run:                              
  command: java -jar target/MyApp-1.0-SNAPSHOT.jar -Xms256m .
```

### Extended Corretto 11 configuration file<a name="service-source-code-java.examples.extended"></a>

This example shows how you can use all the configuration keys with a Corretto 11 managed runtime\.

**Example apprunner\.yaml**  

```
version: 1.0
runtime: corretto11
build:
  commands:
    pre-build:
      - yum install some-package
      - scripts/prebuild.sh
    build:
      - mvn clean package
    post-build:
      - mvn clean test
  env:
    - name: M2
      value: "/usr/local/apache-maven/bin"
    - name: M2_HOME
      value: "/usr/local/apache-maven/bin"
run:
  runtime-version: 11.0.13.08.1
  command: java -jar target/MyApp-1.0-SNAPSHOT.jar -Xms256m .
  network:
    port: 8000
    env: APP_PORT
  env:
    - name: MY_VAR_EXAMPLE
      value: "example"
```

### Complete Corretto 11 application source<a name="service-source-code-java.examples.end2end"></a>

This example shows the source code for a complete Java application that you can deploy to a Corretto 11 runtime service\.

**Example src/main/java/com/HelloWorld/HelloWorld\.java**  

```
package com.HelloWorld;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloWorld {

    @RequestMapping("/")
    public String index(){
        String s = "Hello World";
        return s;
    }
}
```

**Example src/main/java/com/HelloWorld/Main\.java**  

```
package com.HelloWorld;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Main {

    public static void main(String[] args) {

        SpringApplication.run(Main.class, args);
    }
}
```

**Example apprunner\.yaml**  

```
version: 1.0
runtime: corretto11
build:
  commands:    
    build:
      - mvn clean package
run:                              
  command: java -jar target/HelloWorldJavaApp-1.0-SNAPSHOT.jar -Xms256m .
  network:
    port: 8080
```

**Example pom\.xml**  

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.3.1.RELEASE</version>
    <relativePath/>
  </parent>
  <groupId>com.HelloWorld</groupId>
  <artifactId>HelloWorldJavaApp</artifactId>
  <version>1.0-SNAPSHOT</version>

  <properties>
    <java.version>11</java.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-rest</artifactId>
    </dependency>

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
      <scope>test</scope>
      <exclusions>
        <exclusion>
          <groupId>org.junit.vintage</groupId>
          <artifactId>junit-vintage-engine</artifactId>
        </exclusion>
      </exclusions>
    </dependency>
  </dependencies>

  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.8.0</version>
        <configuration>
          <release>11</release>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```