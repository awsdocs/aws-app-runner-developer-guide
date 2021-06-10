# App Runner configuration file reference<a name="config-file-ref"></a>

**Note**  
Configuration files are applicable only to [services that are based on source code](service-source-code.md)\. You can't use configuration files with [image\-based services](service-source-image.md)\.

This topic is a comprehensive reference guide to the syntax and semantics of an AWS App Runner configuration file\. For an overview of App Runner configuration files, see [Setting App Runner service options using a configuration file](config-file.md)\.

The App Runner configuration file is a YAML file\. Name it `apprunner.yaml`, and place it in the root directory of your application’s repository\.

## Structure overview<a name="config-file-ref.overview"></a>

The App Runner configuration file is a YAML file\. Name it `apprunner.yaml`, and place it in the root directory of your application’s repository\.

The App Runner configuration file contains these main parts:
+ *Top section* – Contains top\-level keys
+ *Build section* – Configures the build stage
+ *Run section* – Configures the runtime stage

## Top section<a name="config-file-ref.top"></a>

The keys at the top of the file provide general information about the file and your service runtime\. The following keys are available:
+ `version` – *Required\.* The App Runner configuration file version\. Ideally, use the latest version\.

  **Syntax**

  ```
  version: version
  ```  
**Example**  

  ```
  version: 1.0
  ```
+ `runtime` – *Required\.* The name of the runtime that your application uses\. The following runtimes are currently available:

  `python3`, `nodejs12`
**Note**  
 The naming convention of a managed runtime is *<language\-name><major\-version>*\. 

  **Syntax**

  ```
  runtime: runtime-name
  ```  
**Example**  

  ```
  runtime: python3
  ```

## Build section<a name="config-file-ref.build"></a>

The build section configures the build stage of the App Runner service deployment\. You can specify build commands and environment variables\. Build commands are required\.

The section starts with the `build:` key, and has the following subkeys:
+ `commands` – *Required\.* Specifies the commands that App Runner runs during various build phases\. Includes the following subkeys:
  + `pre-build` – *Optional\.* The commands that App Runner runs before the build\. For example, install npm dependencies or test libraries\.
  + `build` – *Required\.* The commands that App Runner runs to build your application\. For example, use pipenv\.
  + `post-build` – *Optional\.* The commands that App Runner runs after the build\. For example, use Maven to package build artifacts into a JAR or WAR file, or run a test\.

  **Syntax**

  ```
  build:
    commands:
      pre-build:
        - command
        - …
      build:
        - command
        - …
      post-build:
        - command
        - …
  ```  
**Example**  

  ```
  build:
    commands:
      pre-build:
        - yum install openssl
      build:
        - pip install -r requirements.txt
      post-build:
        - python manage.py test
  ```
+ `env` – *Optional\.* Specifies custom environment variables for the build stage\. Defined as name\-value scalar mappings\. You can refer to these variables by name in your build commands\.

  **Syntax**

  ```
  build:
    env:
      - name: name1
        value: value1
      - name: name2
        value: value2
      - …
  ```  
**Example**  

  ```
  build:
    env:
      - name: DJANGO_SETTINGS_MODULE
        value: "django_apprunner.settings"
      - name: MY_VAR_EXAMPLE
        value: "example"
  ```

## Run section<a name="config-file-ref.run"></a>

The run section configures the container running stage of the App Runner application deployment\. You can specify runtime version, start command, network port, and environment variables\.

The section starts with the `run:` key, and has the following subkeys:
+ `runtime-version` – *Optional\.* Specifies a runtime version that you want to lock for your App Runner service\.

  By default, only the major version is locked\. App Runner uses the latest minor and patch versions that are available for the runtime on every deployment or service update\. If you specify major and minor versions, both become locked, and App Runner updates only patch versions\. If you specify major, minor, and patch versions, your service is locked on a specific runtime version and App Runner never updates it\.

  **Syntax**

  ```
  run:
    runtime-version: major[.minor[.patch]]
  ```  
**Example**  

  ```
  runtime: python3
  run:
    runtime-version: 3.7
  ```
+ `command` – *Required\.* The command that App Runner uses to run your application after it completes the application build\.

  **Syntax**

  ```
  run:
    command: command
  ```
+ `network` – *Optional\.* Specifies the port that your application listens to\. It includes the following:
  + `port` – *Optional\.* If specified, this is the port number that your application listens to\. The default is `8080`\.
  + `env` – *Optional\.* If specified, App Runner passes the port number to the container in this environment variable, in addition to \(not instead of\) passing the same port number in the default environment variable, `PORT`\. In other words, if you specify `env`, App Runner passes the port number in two environment variables\.

  **Syntax**

  ```
  run:
    network:
      port: port-number
      env: env-variable-name
  ```  
**Example**  

  ```
  run:
    network:
      port: 8000
      env: MY_APP_PORT
  ```
+ `env` – *Optional\.* Definition of custom environment variables for the run stage\. Defined as name\-value scalar mappings\. You can refer to these variables by name in your runtime environment\.

  **Syntax**

  ```
  run:
    env:
      - name: name1
        value: value1
      - name: name2
        value: value2
      - …
  ```  
**Example**  

  ```
  run:
    env:
      - name: MY_VAR_EXAMPLE
        value: "example"
  ```