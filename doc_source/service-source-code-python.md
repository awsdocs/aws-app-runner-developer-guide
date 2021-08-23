# Using the Python managed runtime<a name="service-source-code-python"></a>

AWS App Runner provides a Python managed runtime\. The runtime makes it easy to build and run containers with Python based web applications\. When you use the Python runtime, App Runner starts with a managed Python runtime image\. This image is based on the [Amazon Linux Docker image](https://hub.docker.com/_/amazonlinux) and contains the Python runtime package and some tools and popular dependency packages\. App Runner uses this managed runtime image as a base image, and adds your application code to build a Docker image\. It then deploys this image to run your web service in a container\.

 You specify a runtime for your App Runner service when you [create a service](manage-create.md) using the App Runner console or the [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) API\. You can also specify a runtime as part of your source code\. Use the `runtime` keyword in a [App Runner configuration file](config-file.md) that you include in your code repository\. The naming convention of a managed runtime is *<language\-name><major\-version>*\. 

For valid Python runtime names, see [Python runtime release information](service-source-code-python-releases.md)\.

App Runner updates the runtime for your service to the latest version on every deployment or service update\. If your application requires a specific version of a managed runtime, you can specify it using the `runtime-version` keyword in the [App Runner configuration file](config-file.md)\. Specify a minor version as *<major>*\.*<minor>* to lock the major and minor versions \(App Runner updates only patch versions\)\. Specify a particular patch level as *<major>*\.*<minor>*\.*<patch>* to lock your service on a specific runtime version \(App Runner never updates the runtime\)\.

**Topics**
+ [Python runtime configuration](#service-source-code-python.config)
+ [Python runtime examples](#service-source-code-python.examples)
+ [Python runtime release information](service-source-code-python-releases.md)

## Python runtime configuration<a name="service-source-code-python.config"></a>

When you choose a managed runtime, you must also configure, as a minimum, build and run commands\. You configure them while [creating](manage-create.md) or [updating](manage-configure.md) your App Runner service\. There are a few ways to do it:
+ **Using the App Runner console** – Specify the commands in the **Configure build** section of the creation process or configuration tab\.
+ **Using the App Runner API** – Call [CreateService](https://docs.aws.amazon.com/apprunner/latest/api/API_CreateService.html) or [UpdateService](https://docs.aws.amazon.com/apprunner/latest/api/API_UpdateService.html)\. Specify the commands using the `BuildCommand` and `StartCommand` members of the [CodeConfigurationValues](https://docs.aws.amazon.com/apprunner/latest/api/API_CodeConfigurationValues.html) data type\.
+ **Using a [configuration file](config-file.md)** – Specify one or more build commands in up to three build phases, and a single run command that serves to start your application\. There are additional optional configuration settings\.

Providing a configuration file is optional\. When creating an App Runner service using the console or the API, you specify if App Runner gets your configuration settings directly during creation or from a configuration file\.

## Python runtime examples<a name="service-source-code-python.examples"></a>

The following examples show App Runner configuration files for building and running a Python service\. The last example is the source code for a complete Python application that you can deploy to a Python runtime service\.

### Minimal Python configuration file<a name="service-source-code-python.examples.minimal"></a>

This example shows a minimal configuration file that you can use with the Python managed runtime\. For the assumptions that App Runner makes with a minimal configuration file, see [Configuration file examples](config-file-examples.md#config-file-examples.managed)\.

**Example apprunner\.yaml**  

```
version: 1.0
runtime: python3 
build:
  commands:
    build:
      - pip install pipenv
      - pipenv install 
run: 
  command: python app.py
```

### Extended Python configuration file<a name="service-source-code-python.examples.extended"></a>

This example shows the use of all configuration keys with the Python managed runtime\.

**Example apprunner\.yaml**  

```
version: 1.0
runtime: python3 
build:
  commands:
    pre-build:
      - wget -c https://s3.amazonaws.com/DOC-EXAMPLE-BUCKET/test-lib.tar.gz -O - | tar -xz
    build:        
      - pip install pipenv
      - pipenv install
    post-build:
      - python manage.py test
  env:
    - name: DJANGO_SETTINGS_MODULE
      value: "django_apprunner.settings"
    - name: MY_VAR_EXAMPLE
      value: "example"
run:
  runtime-version: 3.7.7
  command: pipenv run gunicorn django_apprunner.wsgi --log-file -
  network: 
    port: 8000
    env: MY_APP_PORT  
  env:
    - name: MY_VAR_EXAMPLE
      value: "example"
```

### End to end Python application source<a name="service-source-code-python.examples.end2end"></a>

This example shows the source code for a complete Python application that you can deploy to a Python runtime service\.

**Example requirements\.txt**  

```
pyramid==2.0
```

**Example server\.py**  

```
from wsgiref.simple_server import make_server
from pyramid.config import Configurator
from pyramid.response import Response
import os

def hello_world(request):
    name = os.environ.get('NAME')
    if name == None or len(name) == 0:
        name = "world"
    message = "Hello, " + name + "!\n"
    return Response(message)

if __name__ == '__main__':
    port = int(os.environ.get("PORT"))
    with Configurator() as config:
        config.add_route('hello', '/')
        config.add_view(hello_world, route_name='hello')
        app = config.make_wsgi_app()
    server = make_server('0.0.0.0', port, app)
    server.serve_forever()
```

**Example apprunner\.yaml**  

```
version: 1.0
runtime: python3
build:
  commands:
    build:
      - pip install -r requirements.txt
run:
  command: python server.py
```