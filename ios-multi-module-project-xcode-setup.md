# About

This guide provides a step-by-step approach in order to create a modular iOS App, convering from creating the app itself to creating modules and adding these modules to the app.

The projects will use Xcode for each module in the app.

Convering from creating the app itself to creating modules and adding these modules to the app.

# Conventions

* ***GIT_URL***: URL for git repository.
* ***REPOSITORY_ROOT_FOLDER***: Checkout path for ***GIT_URL***.
* ***APP_FOLDER***: The folder where the App project is. Will be created inside ***REPOSITORY_ROOT_FOLDER***.
* ***MODULES_FOLDER***: Path for the module itself. Will be created inside ***REPOSITORY_ROOT_FOLDER***. In this guide, will be named `modules`
* ***TEMP_FOLDER***: Path for the temporary folder, where temporary artifacts will be created. Will be created inside ***REPOSITORY_ROOT_FOLDER***. In this guide, will be named `temp`.

## Example:

* ***GIT_URL***: git@github.com:inacioferrarini/pipoca.git
* ***REPOSITORY_ROOT_FOLDER***: ~/Developer/iOS/pipoca
* ***APP_FOLDER***: ~/Developer/iOS/pipoca/pipoca-app
* ***MODULES_FOLDER***: ~/Developer/iOS/pipoca/pipoca-app/modules
* ***TEMP_FOLDER***: ~/Developer/iOS/pipoca/pipoca-app/temp

# Requirements

[Swift Lint](swift-lint-setup.md) - Source code structure validation.

# Project Setup

## Step 1: Git Repository Setup

Create a repository to host it. Follow the instructions from the project host for its initial setup.

> **Notes:**
> 1. It is handy to init the repository with a README.MD file. This guide will add required files.
> 1. The default branch will be *master*. Is recommended to change it to *develop*.

## Step 2: Creating the Project

Create the project on Xcode, inside ***REPOSITORY_ROOT_FOLDER***. The project location will be refered as ***APP_FOLDER***.

## Step 3: Gitignore Setup

Add, inside ***REPOSITORY_ROOT_FOLDER***:

```bash
curl -O https://raw.githubusercontent.com/inacioferrarini/step-by-step/master/resources/.gitignore
```

## Step 4: Gemfile Setup

Add, inside ***REPOSITORY_ROOT_FOLDER***:

```bash
curl -O https://raw.githubusercontent.com/inacioferrarini/step-by-step/master/resources/Gemfile
```

## Step 5: Ruby Version Setup

Add, inside ***REPOSITORY_ROOT_FOLDER***:

```bash
curl -O https://raw.githubusercontent.com/inacioferrarini/step-by-step/master/resources/.ruby-version
```

## Step 6: Jazzy Setup

Add, inside ***APP_FOLDER***:

```bash
curl -O https://raw.githubusercontent.com/inacioferrarini/step-by-step/master/resources/.jazzy.yml
```

## Step 7: Swiftlint

Add inside ***APP_FOLDER***:

```bash
curl -O https://raw.githubusercontent.com/inacioferrarini/step-by-step/master/resources/.swiftlint.yml
```

Add Swiftlint configuration to the build phase:
Add a *Run Script Phase*
```bash
if which swiftlint >/dev/null; then
  swiftlint
else
  echo "warning: SwiftLint not installed, download from https://github.com/realm/SwiftLint"
fi
```

## Step 8: Folder Structure for Modules

Create ***MODULES_FOLDER*** inside ***REPOSITORY_ROOT_FOLDER***, where all modules will be located.

Create ***TEMP_FOLDER*** inside ***REPOSITORY_ROOT_FOLDER***, where all temporary modules will be located.

## Step 9: Creating a Module

A module is used to keep features organized and grouped together, helps to avoid code duplication and allows to isolate features and provide a public-acces entry point to a feature.

Each module will have its own git repository.

### Creating a New Module

Inside ***TEMP_FOLDER*** create a new **Cocoa Touch Framework** using Xcode. You can leave **Create Git Repository on my Mac** checked.

Inside the folder for the newly created module, execute **Jazzy Setup** and **Swiftlint** for the new module.

Once the initial setup for the module is done, make the push to the git repository.

Go to ***MODULES_FOLDER***

```bash
$ git submodule add [GIT_MODULE_URL] [MODULE_NAME]
```

Final Steps:
* Add the new module Framework to the main App in **General** tab.
* Add the new module project to the **Build Phases**, under ***Target Dependencies***. This way, whenever the content of the module changes, it will be rebuild when build the app itself.
