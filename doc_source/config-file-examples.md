# App Runner configuration file examples<a name="config-file-examples"></a>

**Note**  
Configuration files are applicable only to [services that are based on source code](service-source-code.md)\. You can't use configuration files with [image\-based services](service-source-image.md)\.

The following examples demonstrate AWS App Runner configuration files\. Some are minimal and contain only required settings\. Others are complete, including all configuration file sections\. For an overview of App Runner configuration files, see [Setting App Runner service options using a configuration file](config-file.md)\.

## Configuration file examples<a name="config-file-examples.managed"></a>

### Minimal configuration file<a name="config-file-examples.managed.minimal"></a>

With a minimal configuration file, App Runner makes the following assumptions:
+ No custom environment variables are necessary during build or run\.
+ The latest runtime version is used\.
+ The default port number and port environment variable are used\.

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

### Complete configuration file<a name="config-file-examples.managed.complete"></a>

This example shows the use of all configuration keys with a managed runtime\.

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

For examples of specific managed runtime configuration files, see the specific runtime subtopic under [Code\-based service](service-source-code.md)\.