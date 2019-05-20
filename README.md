# Digipost Open Source – Travis Configuration

This repository contains common config for building open source projects with Travis CI.


## Usage

This repository should be included open source repositories like this:

```shell
git submodule add https://github.com/digipost/digipost-open-travis-config .travis
```

This will create the `.travis/` folder inside your repository, with this repository cloned as a [Git submodule](https://github.blog/2016-02-01-working-with-submodules/). This provides a settings.xml file for Maven which enables Travis CI to resolve snapshots from the Sonatype snapshot repository. The `./travis/maven.settings.xml` file should be provided to the Maven build command which is run on Travis CI.

This can be done with the following config in your `.travis.yml` file:

```yaml
before_script:
  - cp .travis/maven.settings.xml ~/.m2/settings.xml
```


## Example: `.travis.yml`

Unless you have some very specific needs, the `.travis.yml` file used to configure how Travis CI builds your project should look very much like this:

```yaml
dist: trusty
language: java
jdk:
- oraclejdk8
- openjdk11    # open source libraries should target Java 8, but
- openjdk12    # it is nice to verify it also builds with later JDKs.

cache:
  directories:
  - $HOME/.m2

env:
  global:
  - secure: "encrypted string"
  - secure: "encrypted string"

install: true

before_script:
  - cp .travis/maven.settings.xml ~/.m2/settings.xml

script:
  - mvn clean deploy --update-snapshots
```
