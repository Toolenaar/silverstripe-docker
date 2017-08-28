# SilverStripe Development Environment

This repository is designed to be used with SilverStripe development environments. 
It contains packages and functionality commonly used when developing with SilverStripe.

## Maintainer Contact

 * Brett Tasker <brett.tasker@gmail.com>

## Requirements

 * [Docker](https://docs.docker.com/engine/installation/)

### Installation

```bash
composer create-project brettt89/silverstripe-docker my-website
```

This docker-compose setup comes packaged with:

 * Apache Webserver
 * MySQL Database
 * Composer
 * SSPAK

### Setup
##### Clone website into `public` directory
```bash
git clone <repo> public
```
*NOTE: It is important that the website codebase exists in the public/ directory*

##### Install composer dependencies:
```bash
docker-compose run composer install
```

##### Import database with sspak:

*NOTE: Copy your sspak into the `snapshots/` directory*
```bash
docker-compose run sspak load snapshots/<sspak> public
```

##### Finally, bring up the site and the asset builder:
```bash
docker-compose up -d web
```
The asset builder uses gulp to watch the swift directory for any changes and 
rebuilds as necessary

**The site will then be available at http://localhost/.**

### Additional Configurations

#### Node / NPM / Gulp / etc

To add Node and NPM based applications to you environment, add the following to the 
docker-compose.yml file.

```yml
assets:
  image: node
  volumes:
    - './public/<packages.json location>:/usr/src/app'
  working_dir: '/usr/src/app'
```

*Make sure to replace <packages.json location> with the location of your packages.json file.
E.g. `./public/themes/my-theme:/usr/src/app`*

You can then run you NPM / Node based commands like so
```bash
# Install NPM modules
docker-compose run assets npm install

# Run Gulp from NPM installed gulp
docker-compose run assets node_modules/.bin/gulp

# Have Gulp watch for changes and build as necessary
docker-compose run assets node_modules/.bin/gulp watch
```

You could add a command to the docker-compose.yml file for node to always execute commands when
started.

```yml
assets:
  image: node
  volumes:
    - './public/<packages.json location>:/usr/src/app'
  working_dir: '/usr/src/app'
  command:
    'bash -c "npm install && node_modules/.bin/gulp watch"'
```

Then you can just add "assets" to your `docker-compose up` command
```bash
docker-compose up -d web assets
```

This will run `npm install` to install any NPM packages (if not already installed) and the run 
`gulp watch` in the background to generate assets as files are changed.