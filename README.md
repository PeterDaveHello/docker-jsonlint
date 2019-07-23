# docker-jsonlint

[![Build Status](https://travis-ci.com/PeterDaveHello/docker-jsonlint.svg?branch=master)](https://travis-ci.com/PeterDaveHello/docker-jsonlint)
[![Docker Hub pulls](https://img.shields.io/docker/pulls/peterdavehello/jsonlint.svg)](https://hub.docker.com/r/peterdavehello/jsonlint/)
[![Docker image layers](https://images.microbadger.com/badges/image/peterdavehello/jsonlint.svg)](https://microbadger.com/images/peterdavehello/jsonlint/)
[![Docker image version](https://images.microbadger.com/badges/version/peterdavehello/jsonlint.svg)](https://hub.docker.com/r/peterdavehello/jsonlint/tags/)

[![Docker Hub badge](http://dockeri.co/image/peterdavehello/jsonlint)](https://hub.docker.com/r/peterdavehello/jsonlint/)

Dockerized [jsonlint](https://github.com/zaach/jsonlint) with various versions, easy to use and easy to integrate with CI.

## Table of Contents

- [Usage](#usage)
  - [Command line](#command-line)
    - [Use latest version](#use-latest-version)
    - [Use specific version](#use-specific-version)
  - [Continuous Integration (CI)](#continuous-integration-ci)
    - [Travis CI](#travis-ci)
    - [GitLab CI](#gitlab-ci)
- [jsonlint cli usage](#jsonlint-cli-usage)
- [Build](#build)

## Usage

### Command line

#### Use latest version

```sh
docker run --rm -v $PATH_TO_JSON:/json peterdavehello/jsonlint jsonlint -q JSON_FILE.json

# Please replace "$PATH_TO_JSON" with your custom path,
# and replace "JSON_FILE.json" with your real json file filename.
```

#### Use specific version

Just like above, but you can specify version of jsonlint, for example:

```sh
docker run --rm -v $PATH_TO_JSON:/json peterdavehello/jsonlint:1.6.3 jsonlint -q JSON_FILE.json

# Please replace "1.6.3" with the version number you want.
# Don't forget to replace "$PATH_TO_JSON" & "JSON_FILE.json".
```

### Continuous Integration (CI)

#### Travis CI

Enable Docker service in your `.travis.yml`:

```yaml
services:
  - docker
```

And use the same command in the `scripts` part as the command line mentions, for example:

```yaml
services:
  - docker

scripts:
  - docker run --rm -v $TRAVIS_BUILD_DIR:/json peterdavehello/jsonlint:1.6.3 jsonlint -q example.json
```

This will lint a example json file called `example.json`

#### GitLab CI

Add this block to your `.gitlab-ci.yml`:

```yaml
jsonlint:
  stage: lint
  variables:
    jsonlint_version: "1.6.3"
  image: peterdavehello/jsonlint:$jsonlint_version
  only:
    changes:
      - "**/*.json"
  script:
    - find . -name "*.json" | xargs -n 1 jsonlint -q
```

Replace "1.6.3" with the version you want to use, you can also use "latest" for the very new version.

## jsonlint cli usage

Just pass `-h`/`--help` to jsonlint to get its help message, for example:

```sh
$ docker run --rm peterdavehello/jsonlint jsonlint --help

Usage: jsonlint [file] [options]

file     file to parse; otherwise uses stdin

Options:
   -v, --version            print version and exit
   -s, --sort-keys          sort object keys
   -i, --in-place           overwrite the file
   -t CHAR, --indent CHAR   character(s) to use for indentation  [  ]
   -c, --compact            compact error display
   -V, --validate           a JSON schema to use for validation
   -e, --environment        which specification of JSON Schema the validation file uses  [json-schema-draft-03]
   -q, --quiet              do not print the parsed json to STDOUT  [false]
```

For more details, check out the [jsonlint project](https://github.com/zaach/jsonlint) page.

## Build

Build command, you need to specify a valid jsonlint version argument to `jsonLINT_VERSION`:

```sh
docker build --build-arg JSONLINT_VERSION="1.6.3" -t docker-jsonlint .

# Replace "docker-jsonlint" with the preferred image name
```

You can find a valid version on [jsonlint](https://www.npmjs.com/package/jsonlint?activeTab=versions) npm registry page, or just poke the [registry](https://registry.npmjs.org/jsonlint) to retrieve more details.
