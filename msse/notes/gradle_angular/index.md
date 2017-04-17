---
title: Web Application Development
layout: default
---
slidenumbers: true

## Angular with Spring Boot and Gradle

### Marc Kapke
#### kapkema@gmail.com

---

# Goals
 - Add Angular frontend to Spring Boot app
 - Introduce tools for dependency management, building, testing
 - Run Angular build and tests from `./gradlew build`

---

# Required Tools to manage Angular project
- Node
- Node Package Manager (NPM)
- angular-cli

---

# Node
- Node supports modular JavaScript application development
- Designed to be a self-standing (non-browser) platform
- Single-threaded event loop
- Node supports a package model for installing utilities and Node-based tools
- Client frameworks (Angular, etc...) use Node solely to access Node Package Manager

---

# Node Package Manager (NPM)
- Part of the Node installation
- The only part of Node truly required for client-side development
- `npm install <package_name>`
  - Tools can be installed globally
  - Local dependencies managed in `node_modules` directory

---

# Package
- A package is a Node module
- Directory with one or more files
- Includes `package.json` manifest file in root
- See available packages at [npmjs.com](http://www.npmjs.com)

---

# package.json
- Defines metadata about module
- Lists runtime dependencies
- Lists developer dependencies
- Defines build tasks
- Don't hand author

---

# Creating new NPM module and `package.json`
- Initialize the module:
  - `npm init -f`
- This creates `package.json` in working directory

---

# Adding dependencies to `package.json`
- Add new runtime dependency:

  ```
  npm install lodash --save
  ```

- Add new developer dependency

  ```
  npm install karma --save-dev
  ```

---

# Example package.json file

``` javascript
{
  "name": "nyt",
  "version": "0.0.1",
  "license": "MIT",
  "scripts": {    
    "build": "ng build",
    ...
  },
  "dependencies": {
    "@angular/common": "^2.4.0",
    ...
  },
  "devDependencies": {
    "@angular/cli": "1.0.0-rc.2",
    ...
  }
}
```

---

# NPM - build tasks
- NPM also includes the ability to define build tasks
- Use `scripts` block in `package.json`

``` javascript
"scripts": {
   "prebuild": "echo about to build!",
   "build": "ng build",
   "postbuild": "echo done building!"   
 },
```
- Each build task has 3 states that can be utilized
  - `pre[TaskName]`, `[TaskName]`, `post[TaskName]`

---

# Pre-defined tasks
- `build`, `test`, `publish`, etc..
- Can utilize pre/post hooks by naming convention
  - `prebuild`, `build`, `postbuild`
- command: `npm [taskName]`
- Available pre-defined build tasks: [docs.npmjs.com/misc/scripts](https://docs.npmjs.com/misc/scripts)

---


# Custom tasks
- Define new tasks to support your module
- Custom tasks must include `run` in command
  - command: `npm run hello`
- Utilize pre/post hooks by naming convention
  - `prehello`, `hello`, `posthello`

---

# Custom task definition

``` javascript
"scripts": {
  ...
  "hello": "echo hello",
  "prehello": "echo I'm executing before hello ",
  "posthello": "echo I'm executing after hello "
},
```

---

# NPM for Angular
- Dependency management and Build tasks in one place
- Provides familiar build pipeline for javascript

---

# `@angular\cli`
- NPM package installed globally
- available as `ng` on command line
- Bootstraps Angular project similar to [Spring Initializr](https://start.spring.io)

---

# What does `@angular\cli` do?
- Generates necessary NPM module definition, including `package.json`
- Generates all necessary config, source and test files for 'Hello world' app
- Generates build pipeline capable of building, testing and running app
- Supporting `ng` tasks configured in `.angular-cli.json`

---

# Install Angular required dependencies
- Node and NPM
 - Download from [nodejs.org](http://nodejs.org)
 - Or use `brew`, `nvm` or equivalent
- Install `@angular/cli` globally via NPM
  - `npm install -g @angular/cli`

---

# Add Angular to Spring Boot project
- Create `javascript` directory under `/src/main/`
- From `javascript` directory run:

 ```
 ng new --skip-git --directory app [app-name]
 ```

    - `skip-git` - don't init git repo in 'javascript'.
    - `--directory` is path under `javascript` where src is generated.
        - In this example it will be `/src/main/javascript/app`
    - replace `[app-name]` with the relevant app name referenced in metadata

---

# Modify `.angular-cli.json`
- Update `angular-cli.json` to put resources where Spring Boot can serve them
- `outDir` needs to be relative path to `/src/main/resources/static`

``` javascript
"apps": [
    {
      "root": "src",
      "outDir": "../../resources/static",
      ...
    }]
```

---

# Update `.gitignore`
- Don't include non-source code in repository
- Add these to your `.gitignore`

```
node_modules
jspm_packages
npm-debug.log
debug.log
_test-output
_temp
/node/
/src/main/resources/static
```

---

# Enable Gradle to run NPM commands
- In `build.gradle` add Node/NPM plugin

``` groovy
plugins {
  id "com.moowork.node" version "1.1.1"
}
```

---

# Configure Node/NPM Plugin
``` groovy
node {
  version = '7.7.3'
  npmVersion = '4.1.2'
  download = true
  workDir = file("${project.projectDir}/node")
  nodeModulesDir = file("${project.projectDir}/src/main/javascript/app")
  npmWorkDir = file("${project.buildDir}/src/main/javascript/app")
}
```

---

# Configure Node/NPM Plugin
- For repeatable builds utilize specific versions of Node and NPM
- Set `download = true` to always download and use specified versions
- `nodeModulesDir` and `npmWorkDir` should point to folder containing `package.json`
- `workDir` is where plugin will download node binaries to

---
# Running NPM Tasks from Gradle
- Once configured, any `npm` task can be accessed in plugin
- To run from gradle replace npm command spaces with underscores
  - `npm run build` == `./gradlew npm_run_build`

---

# Setup Gradle Build task
- Gradle's `processResources` copies compiled resources to target
- Before `processResources` starts, build Angular so its resources copied too.
- `npm run build` builds Angular - link it to existing task `processResources`
- `npm run build` requires `npmInstall` before it can run

---

# Gradle build task mapping

``` groovy
npm_run_test.dependsOn npmInstall
npm_run_build.dependsOn npm_run_test

processResources.dependsOn npm_run_build
npm_run_start.dependsOn npm_run_build
```

---

# Clean up after yourself
- Define what to do when `clean` runs
- Remove all non-source required to build

```
clean {
	final def webDir = "${project.projectDir}"
	delete "${webDir}/node"
	delete "${webDir}/src/main/javascript/app/node_modules"
	delete "${webDir}/src/main/resources/static"
	delete "${webDir}/coverage"
}
```

---

# Enable proxy calls to Spring Boot
- Add `proxy.config.json` in same directory as `package.json`

``` javascript
{
  "/api/*": {
    "target": "http://localhost:8080/",
    "secure": false
  }
}
```

---

# Update `npm start`
- Include proxy.config when running `npm start`
- This is defined in `package.json`

```
"scripts": {
    "ng": "ng",
    "start": "ng serve --proxy-config proxy.config.json",
    "build": "ng build",
    "test": "ng test --watch false",
    "test-watcher": "ng test"
  },
```


---

# Putting it all together
- Run(Debug) your SpringBoot server
- From your root directory run `./gradlew npm_run_start`
- Access front-end from `http://localhost:4200`
