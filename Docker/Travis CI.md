## Travis CI – Overview

Travis CI is a cloud-based Continuous Integration and Continuous Deployment (CI/CD) service that automatically builds, tests, and deploys your application when code changes are pushed to a Git repository like GitHub or GitLab.

Its main goal is to catch bugs early, ensure code quality, and automate deployment workflows.

Key ideas behind Travis CI

- Every push or pull request triggers an automated pipeline
- The pipeline is defined as code in a `.travis.yml` file
- Builds run in clean, isolated environments
- Supports multiple languages, Docker, and cloud deployments
- Integrates easily with AWS, Docker Hub, npm, PyPI, etc.

Typical workflow

1. Developer pushes code to repository
2. Travis CI detects the change
3. Travis reads `.travis.yml`
4. Dependencies are installed
5. Tests and build steps run
6. Optional deployment is triggered if conditions match

## What is `.travis.yml`

`.travis.yml` is a YAML configuration file placed at the root of your repository.  
It defines how Travis CI should build, test, and deploy your project.

Without this file, Travis CI does nothing.

## Basic Structure of `.travis.yml`

```yaml
language: node_js

node_js:
  - "20"

install:
  - npm install

script:
  - npm test
```

This tells Travis to

- Use Node.js
- Install dependencies
- Run tests

## Core Sections Explained

### `language`

Specifies the main programming language.

Examples

```yaml
language: node_js
language: python
language: java
language: generic
```

Use `generic` when working mainly with Docker.

### Runtime Version

Defines versions of the language or runtime.

Node.js

```yaml
node_js:
  - "18"
  - "20"
```

Python

```yaml
python:
  - "3.11"
```

### `os` and `dist`

Choose operating system and Linux distribution.

```yaml
os: linux
dist: jammy
```

Common dist values

- xenial
- bionic
- focal
- jammy

### `services`

Used to enable services like Docker, Redis, PostgreSQL.

```yaml
services:
  - docker
  - redis-server
```

### `before_install`

Runs commands before dependency installation.

```yaml
before_install:
  - sudo apt-get update
```

### `install`

Installs dependencies.

```yaml
install:
  - npm ci
```

If omitted, Travis may use default install behavior.

### `script`

The most important section  
Defines what commands must succeed for the build to pass.

```yaml
script:
  - npm run lint
  - npm test
  - npm run build
```

If any command fails, the build fails.

### `after_success` and `after_failure`

Run commands based on build outcome.

```yaml
after_success:
  - echo "Build passed"

after_failure:
  - echo "Build failed"
```

### `env`

Environment variables.

Inline

```yaml
env:
  - NODE_ENV=test
```

Matrix

```yaml
env:
  matrix:
    - NODE_ENV=test
    - NODE_ENV=production
```

Encrypted secrets are used for tokens and keys.

### Encrypted Environment Variables

Used for AWS keys, Docker Hub tokens, etc.

```bash
travis encrypt AWS_SECRET_ACCESS_KEY=xxxx
```

Then in `.travis.yml`

```yaml
env:
  secure: "encrypted_value_here"
```

## Build Matrix

Allows testing across multiple environments.

```yaml
node_js:
  - "18"
  - "20"

os:
  - linux
  - osx
```

Travis will run all combinations.

## Docker-Based Builds

Common for modern applications.

```yaml
language: generic

services:
  - docker

script:
  - docker build -t myapp .
  - docker run myapp npm test
```

## Caching for Faster Builds

```yaml
cache:
  npm: true
```

Or directories

```yaml
cache:
  directories:
    - node_modules
```

## Conditional Builds

Run builds only on specific branches.

```yaml
branches:
  only:
    - main
```

Or exclude

```yaml
branches:
  except:
    - experimental
```

## Deployment Section

Example for AWS Elastic Beanstalk

```yaml
deploy:
  provider: elasticbeanstalk
  region: us-east-1
  app: my-app
  env: my-app-env
  bucket_name: my-bucket
  on:
    branch: main
```

Docker Hub example

```yaml
deploy:
  provider: script
  script: docker push myimage
  on:
    branch: main
```

## Best Practices for Writing `.travis.yml`

- Keep builds deterministic using `npm ci` or locked dependencies
- Use caching to reduce build time
- Never hardcode secrets
- Use branch conditions for production deployment
- Prefer Docker for consistency across dev, test, and prod
- Keep scripts short and readable
- Fail fast by putting critical checks early

## Common Mistakes

- Incorrect YAML indentation
- Forgetting to enable Travis CI for the repository
- Missing `script` section
- Using deprecated Linux distributions
- Exposing secrets in plain text
- Running heavy builds without caching

## Minimal Real-World Example (Node.js + Docker)

```yaml
language: generic

services:
  - docker

before_install:
  - docker build -t myapp .

script:
  - docker run myapp npm test

deploy:
  provider: script
  script: docker push mydockerhub/myapp
  on:
    branch: main
```

