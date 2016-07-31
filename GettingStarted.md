# Structure

## General

### Source

### Specifications

### Modules

### Distribution

## JavaScript

## CSharp

# Cloning and getting started

All you need to 

```terminal
npm install
```


## Submodules

Cratis uses GIT submodules during development to make the developer experience a bit better.
This is only for some developer dependencies, like the JavaScript build pipeline for instance. 
The reason why it is a submodule rather than a package in NPM or elsewhere, is that during the 
initial setup of the projects, having a reused build pipeline easily got out of hand with dependencies and
trouble at runtime. With it being a submodule in combination with the folder structure, we are
able to reuse the node_modules at the root level, since NPM will in fact look up the hierarchy. 
The submodules are only downloaded as is - they are not initialized in-place.

From this we get to reuse build files and setting files that makes it real easy to develop new
modules to the Cratis platform. The NPM package description has a `preinstall` script that deals with
this. It performs the following:

```terminal
git submodule update --init --recursive
cd Modules/JavaScript
git checkout master
```

## JSPM

Cratis uses the [JavaScript Package Manager](http://jspm.io) for handling package dependencies that 
are used at runtime. Developer dependencies are being handled by NPM. 

# Running tests

## Wallaby

## Karma

