# SilverStripe Development Environment

This repository is designed to be used with SilverStripe development environments. 
It contains packages and functionality commonly used when developing with SilverStripe.

## Maintainer Contact

 * Brett Tasker <brett.tasker@gmail.com>

## Requirements

 * [Docker](https://docs.docker.com/engine/installation/)

### Installation

```bash
composer create-project brettt89/silverstripe-docker ./my/website/folder
```

`./my/website/folder` should be the root directory where your codebase and configuration 
files will live.

##### Applications packaged in docker-compose.yml

 * Apache Webserver
 * MySQL Database
 * Composer
 * SSPAK

### Setup

These commands should be run from within the project folder created during Installation.

*E.g. `./my/website/folder`*

##### Clone website into `public/` directory
```bash
git clone <repo> public
```
*NOTE: It is important that the website codebase exists in the `public/` directory*

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