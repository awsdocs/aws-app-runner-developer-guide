# Using the Node\.js platform<a name="service-source-code-nodejs"></a>

The AWS App Runner Node\.js platform provides managed runtimes\. Each runtime makes it easy to build and run containers with web applications based on a Node\.js version\. When you use a Node\.js runtime, App Runner starts with a managed Node\.js runtime image\. This image is based on the [Amazon Linux Docker image](https://hub.docker.com/_/amazonlinux) and contains the runtime package for a version of Node\.js and some tools\. App Runner uses this managed runtime image as a base image, and adds your application code to build a Docker image\. It then deploys this image to run your web service in a container\.

 You specify a runtime for your App Runner service when you [create a service](manage-create.md) using the App Runner console or the [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) API\. You can also specify a runtime as part of your source code\. Use the `runtime` keyword in a [App Runner configuration file](config-file.md) that you include in your code repository\. The naming convention of a managed runtime is *<language\-name><major\-version>*\. 

For valid Node\.js runtime names, see [Node\.js runtime release information](service-source-code-nodejs-releases.md)\.

App Runner updates the runtime for your service to the latest version on every deployment or service update\. If your application requires a specific version of a managed runtime, you can specify it using the `runtime-version` keyword in the [App Runner configuration file](config-file.md)\. You can lock to any level of version \(for example, major or minor\), and App Runner only makes lower\-level updates to the runtime of your service\.

Version syntax for Node\.js runtimes: `major[.minor[.patch]]`

For example: `12.22.6`

The following examples demonstrate version locking:
+ `12.22` – Lock the major and minor versions\. App Runner updates only patch versions\.
+ `12.22.3` – Lock to a specific patch version\. App Runner doesn't update your runtime version\.

**Topics**
+ [Node\.js runtime configuration](#service-source-code-nodejs.config)
+ [Node\.js runtime examples](#service-source-code-nodejs.examples)
+ [Node\.js runtime release information](service-source-code-nodejs-releases.md)

## Node\.js runtime configuration<a name="service-source-code-nodejs.config"></a>

When you choose a managed runtime, you must also configure, as a minimum, build and run commands\. You configure them while [creating](manage-create.md) or [updating](manage-configure.md) your App Runner service\. There are a few ways to do it:
+ **Using the App Runner console** – Specify the commands in the **Configure build** section of the creation process or configuration tab\.
+ **Using the App Runner API** – Call [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) or [UpdateService](https://docs.aws.amazon.com/apprunner/latest/api/API_UpdateService.html)\. Specify the commands using the `BuildCommand` and `StartCommand` members of the [CodeConfigurationValues](https://docs.aws.amazon.com/apprunner/latest/api/API_CodeConfigurationValues.html) data type\.
+ **Using a [configuration file](config-file.md)** – Specify one or more build commands in up to three build phases, and a single run command that serves to start your application\. There are additional optional configuration settings\.

Providing a configuration file is optional\. When creating an App Runner service using the console or the API, you specify if App Runner gets your configuration settings directly during creation or from a configuration file\.

With Node\.js runtimes specifically, you can also configure the build and runtime using a JSON file named `package.json` in the root of your source repository\. Using this file, you can configure the Node\.js engine version, dependency packages, and various commands \(command line applications\)\. Package managers such as npm or yarn interpret this file as input for their commands\.

For example:
+ npm install installs packages defined by the `dependencies` and `devDependencies` node in `package.json`\.
+ npm start or npm run start runs the command defined by the `scripts/start` node in `package.json`\.

The following is an example `package.json` file\.

### package\.json<a name="service-source-code-nodejs.config.package-json-example"></a>

```
{
  "name": "node-js-getting-started",
  "version": "0.3.0",
  "description": "A sample Node.js app using Express 4",
  "engines": {
    "node": "12.18.4"
  },
  "scripts": {
    "start": "node index.js",
    "test": "node test.js"
  },
  "dependencies": {
    "cool-ascii-faces": "^1.3.4",
    "ejs": "^2.5.6",
    "express": "^4.15.2"
  },
  "devDependencies": {
    "got": "^11.3.0",
    "tape": "^4.7.0"
  }
}
```

For more information about `package.json`, see [The package\.json guide](https://nodejs.dev/learn/the-package-json-guide) on the Node\.js website\.

**Tips**  
If your `package.json` file defines a start command, you can use it as a run command in your App Runner configuration file, as the following example shows\.  

**Example**  
package\.json  

  ```
  {
    "scripts": {
      "start": "node index.js"
    }
  }
  ```
apprunner\.yaml  

  ```
  run:
    command: npm start
  ```
When you run npm install in your development environment, npm creates the file `package-lock.json`\. This file contains a snapshot of the package versions npm just installed\. Thereafter, when npm installs dependencies, it uses these exact versions\. Similarly, yarn creates `yarn.lock`\. Commit these files to your source code repository to ensure that your application is installed with the versions of dependencies that you developed and tested it with\.
You can also use an App Runner configuration file to configure the Node\.js version and start command\. When you do this, these definitions override the ones in `package.json`\. A conflict between the `node` version in `package.json` and the `runtime-version` value in the App Runner configuration file causes the App Runner build phase to fail\.

## Node\.js runtime examples<a name="service-source-code-nodejs.examples"></a>

The following examples show App Runner configuration files for building and running a Node\.js service\.

### Minimal Node\.js configuration file<a name="service-source-code-nodejs.examples.minimal"></a>

This example shows a minimal configuration file that you can use with a Node\.js managed runtime\. For the assumptions that App Runner makes with a minimal configuration file, see [Configuration file examples](config-file-examples.md#config-file-examples.managed)\.

**Example apprunner\.yaml**  

```
version: 1.0
runtime: nodejs12
build:
  commands:    
    build:
      - npm install --production                                  
run:                              
  command: node app.js
```

### Extended Node\.js configuration file<a name="service-source-code-nodejs.examples.extended"></a>

This example shows the use of all the configuration keys with a Node\.js managed runtime\.

**Example apprunner\.yaml**  

```
version: 1.0
runtime: nodejs12
build:
  commands:
    pre-build:
      - npm install --only=dev
      - node test.js
    build:
      - npm install --production
    post-build:
      - node node_modules/ejs/postinstall.js
  env:
    - name: MY_VAR_EXAMPLE
      value: "example"
run:
  runtime-version: 12.18.4
  command: node app.js
  network:
    port: 8000
    env: APP_PORT
  env:
    - name: MY_VAR_EXAMPLE
      value: "example"
```

### Node\.js app with Grunt<a name="service-source-code-nodejs.examples.grunt"></a>

This example shows how to configure a Node\.js application that's developed with Grunt\. [Grunt](https://gruntjs.com/) is a command line JavaScript task runner\. It runs repetitive tasks and manages process automation to reduce human error\. Grunt and Grunt plugins are installed and managed using npm\. You configure Grunt by including the `Gruntfile.js` file in the root of your source repository\.

**Example package\.json**  

```
{
  "scripts": {
    "build": "grunt uglify",
    "start": "node app.js"
  },
  "devDependencies": {
    "grunt": "~0.4.5",
    "grunt-contrib-jshint": "~0.10.0",
    "grunt-contrib-nodeunit": "~0.4.1",
    "grunt-contrib-uglify": "~0.5.0"
  },
  "dependencies": {
    "express": "^4.15.2"
  },
}
```

**Example Gruntfile\.js**  

```
module.exports = function(grunt) {

  // Project configuration.
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    uglify: {
      options: {
        banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
      },
      build: {
        src: 'src/<%= pkg.name %>.js',
        dest: 'build/<%= pkg.name %>.min.js'
      }
    }
  });

  // Load the plugin that provides the "uglify" task.
  grunt.loadNpmTasks('grunt-contrib-uglify');

  // Default task(s).
  grunt.registerTask('default', ['uglify']);

};
```

**Example apprunner\.yaml**  

```
version: 1.0
runtime: nodejs12
build:
  commands:
    pre-build:
      - npm install grunt grunt-cli
      - npm install --only=dev
      - npm run build
    build:
      - npm install --production
run:
  runtime-version: 12.18.4
  command: node app.js
  network:
    port: 8000
    env: APP_PORT
```
