Heroku buildpack for Ember applications using Fastboot App Server
=================================================================

This buildpack works with Ember CLI generated applications.

The official [Heroku Emberjs buildpack](https://www.heroku.com/emberjs) seems somewhat out of date now and for deploying Fastboot applications in particular.

The use of [Fastboot App Server](https://github.com/ember-fastboot/fastboot-app-server) is now encouraged, where you provide your own Node.js web server.

It's intentionally as simple as possible, hoping to avoid future maintenance issues.

All it really does is run the build process after the node buildpack has finished.

## Usage

### Adding buildpacks

This buildpack is supposed to be run after the default [Heroku Node.js  buildpack](https://github.com/heroku/heroku-buildpack-nodejs) using Heroku [multi buildpack](https://github.com/heroku/heroku-buildpack-multi) that allows one to run multiple buildpacks in a single deploy process.

First set the buildpack to multi

```
$ heroku buildpacks:set https://github.com/heroku/heroku-buildpack-multi.git
```

Then create a `.buildpacks` file which contains (in order) the Node.js buildpack and then this buildpack:
```
$ cat .buildpacks
https://github.com/heroku/heroku-buildpack-nodejs.git
https://github.com/chrism/heroku-buildpack-fastboot-app-server.git
```

### Installing devDependencies

Development dependencies are needed so that the application can be built using Ember CLI after it has been deployed to Heroku. This can be done by setting `NPM_CONFIG_PRODUCTION`.

```
$ heroku config:set NPM_CONFIG_PRODUCTION=false
```

## How it works

Node.js buildpack will install Node.js and npm and then install all Node.js packages (including those specified as development dependencies).

Once installed this buildpack then simply runs...

```
ember build --environment=$EMBER_ENV
```
(by default `EMBER_ENV=production`) to generate your app.

So that the `dist` folder is creating and the asset generated.

Big thanks to [Szymon Nowak's Buildpack](https://github.com/szimek/heroku-buildpack-ember-cli-without-webserver) for the idea and much of the code.
