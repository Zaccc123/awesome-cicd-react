# Awesome CICD React
[![CircleCI](https://circleci.com/gh/Zaccc123/awesome-cicd-react.svg?style=svg)](https://circleci.com/gh/Zaccc123/awesome-cicd-react) [![Code Climate](https://codeclimate.com/github/Zaccc123/awesome-cicd-react/badges/gpa.svg)](https://codeclimate.com/github/Zaccc123/awesome-cicd-react) [![Test Coverage](https://codeclimate.com/github/Zaccc123/awesome-cicd-react/badges/coverage.svg)](https://codeclimate.com/github/Zaccc123/awesome-cicd-react/coverage)

This project aim to show how easy it is to set up a fully automated suites of CI and CD with the new [Create React App](https://github.com/facebookincubator/create-react-app).

## Automated CI & CD Setup
Using the new [Create React App](https://github.com/facebookincubator/create-react-app), the setup of a fully automated CI and CD stack is relatively easy with [CircleCI](https://circleci.com) and [Heroku](https://heroku.com).

### Step 1: Update node to use 6.0.0
As the default `node` version is not >6.0.0 that is required, we have to add a `circle.yml` file in the root directory. The conent is this:

```yml
machine:
  node:
    version: 6.0.0
```

Then head over to [CircleCI](https://circleci.com) to build the project.

### Step 2: Create a Heroku App
Next, we will use a [buildpack](https://github.com/mars/create-react-app-buildpack) to create our heroku app.

```bash
$ heroku create REPLACE_APP_NAME_HERE --buildpack https://github.com/mars/create-react-app-buildpack.git
$ git push heroku master
$ heroku open
```

### Step 3: Setup Automated Deployment
Navigate to the newly create app in [Heroku Dashboard](https://heroku.com).

- Go to `Deploy` tab and `Connect` to the correct github repo.
- Enable Automatic deployment and check `Wait for CI to pass before deploy`

### Thats It.
With just 3 steps, we have a fully automated integration and deployment suites. Now with every commit that is pushed to `GitHub`, it will send a trigger to `CircleCI`. Once the test passed, if it was on the master branch, it will also be automatically deployed to `Heroku`.


View a deployed app [here](https://awesome-cicd-react.herokuapp.com).

This project was bootstrapped with [Create React App](https://github.com/facebookincubator/create-react-app).
