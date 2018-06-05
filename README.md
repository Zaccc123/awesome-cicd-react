# Awesome CICD React

[![CircleCI](https://circleci.com/gh/Zaccc123/awesome-cicd-react.svg?style=svg)](https://circleci.com/gh/Zaccc123/awesome-cicd-react) [![Code Climate](https://codeclimate.com/github/Zaccc123/awesome-cicd-react/badges/gpa.svg)](https://codeclimate.com/github/Zaccc123/awesome-cicd-react) [![Test Coverage](https://codeclimate.com/github/Zaccc123/awesome-cicd-react/badges/coverage.svg)](https://codeclimate.com/github/Zaccc123/awesome-cicd-react/coverage)

This project aim to show how easy it is to set up a fully automated suites of CI and CD with the new [Create React App](https://github.com/facebookincubator/create-react-app).

## Automated CI & CD Setup

Using the new [Create React App](https://github.com/facebookincubator/create-react-app), the setup of a fully automated CI and CD stack is relatively easy with [CircleCI](https://circleci.com), [CodeClimate](https://codeclimate.com) and [Heroku](https://heroku.com).

The full post can be view at this [blog post](https://medium.freecodecamp.org/how-to-set-up-continuous-integration-and-deployment-for-your-react-app-d09ae4525250/).

## TLDR Version

### Step 1: CircleCI 2.0 setup

Create a `.circleci` directory and a `config.yml` file in it. Fill up the content with this:

```yml
version: 2
jobs:
  build:
    docker:
      - image: circleci/node:6
    steps:
      - checkout
      - restore_cache: # special step to restore the dependency cache
          key: dependency-cache-{{ checksum "package.json" }}
      - run:
          name: Setup Dependencies
          command: npm install
      - run:
          name: Setup Code Climate test-reporter
          command: |
            curl -L https://codeclimate.com/downloads/test-reporter/test-reporter-latest-linux-amd64 > ./cc-test-reporter
            chmod +x ./cc-test-reporter
      - save_cache: # special step to save the dependency cache
          key: dependency-cache-{{ checksum "package.json" }}
          paths:
            - ./node_modules
      - run: # run tests
          name: Run Test and Coverage
          command: |
            ./cc-test-reporter before-build
            npm test -- --coverage
            ./cc-test-reporter after-build --exit-code $?
```

Then head over to [CircleCI](https://circleci.com) to build the project.

### Step 2: Setup CodeClimate

Now, head over to [CodeClimate](https://codeclimate.com), sign in and build the created github project. We should get this at the end of analyse:

In order to use `Test Coverage` feedback, we will also need to copy the `Test Reporter ID` from `Settings > Test Coverage` and add it into CircleCI environment variable.

In CircleCI, navigate to `Project > Settings > Environment variable` and add `CC_TEST_REPORTER_ID` with copied `Test Reporter ID`.

### Step 3: Create a Heroku App

Next, we will use a [buildpack](https://github.com/mars/create-react-app-buildpack) to create our heroku app.

```bash
$ heroku create REPLACE_APP_NAME_HERE --buildpack https://github.com/mars/create-react-app-buildpack.git
$ git push heroku master
$ heroku open
```

### Step 4: Setup Automated Deployment

Navigate to the newly create app in [Heroku Dashboard](https://heroku.com).

* Go to `Deploy` tab and `Connect` to the correct github repo.
* Enable Automatic deployment and check `Wait for CI to pass before deploy`

### Thats It.

With just 3 steps, we have a fully automated integration and deployment suites. Now with every commit that is pushed to `GitHub`, it will send a trigger to `CircleCI`. Once the test passed, if it was on the master branch, it will also be automatically deployed to `Heroku`.

View a deployed app [here](https://awesome-cicd-react.herokuapp.com).

This project was bootstrapped with [Create React App](https://github.com/facebookincubator/create-react-app).
