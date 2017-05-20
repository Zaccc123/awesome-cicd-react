# Awesome CICD React

This project was bootstrapped with [Create React App](https://github.com/facebookincubator/create-react-app).

This project aim to show how easy it is to set up a fully automated suites of CI and CD.

## Automated CI & CD Setup
Using the new [Create React App](https://github.com/facebookincubator/create-react-app), the setup of a fully automated CI and CD stack is relatively easy with [CircleCI](https://circleci.com) and [Heroku](https://heroku.com).

### Step 1 Update node to use 6.0.0
As the default `node` version is not >6.0.0 that is required, we have to add a `circle.yml` file in the root directory. The conent is this:

```yml
machine:
  node:
    version: 6.0.0
```

### Step 2 Create a Heroku App
Next, we will use a [buildpack](https://github.com/mars/create-react-app-buildpack) to create our heroku app.

```bash
$ heroku create REPLACE_APP_NAME_HERE --buildpack https://github.com/mars/create-react-app-buildpack.git
$ git push heroku master
$ heroku open
```

### Step 3 Setup Automated Deployment
Navigate to the newly create app in [Heroku Dashboard](https://heroku.com).

- Go to `Deploy` tab and `Connect` to the correct github repo.
- Enable Automatic deployment and check `Wait for CI to pass before deploy`

### Thats It.
With just 3 steps, we have a fully automated integration and deployment suites that will run test before deploying our ReactJS app.
