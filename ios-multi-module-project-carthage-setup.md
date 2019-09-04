# About

This guide provides a step-by-step approach in order to create a modular iOS App, convering from creating the app itself to creating modules and adding these modules to the app.

To share the artifacts, *Carthage* will be used.

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
* ***MODULES_FOLDER***: ~/Developer/iOS/pipoca/modules
* ***TEMP_FOLDER***: ~/Developer/iOS/pipoca/temp

# Requirements

[Carthage](carthage-setup.md) - External dependencies and modules management.

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

## Step 6: Create Carthage Control Files

Add inside ***APP_FOLDER***, a file named ***Cartfile***, with the content:
```bash
github "hackiftekhar/IQKeyboardManager"
```

Add inside ***APP_FOLDER***, a file named ***Cartfile.private***, with the content:
```bash
github "Quick/Quick" "v2.1.0"
github "Quick/Nimble" "v8.0.2"
```

Then, update Carthage

```bash
carthage update --platform iOS --no-use-binaries --cache-builds
```

From Carthage:
On your application targets’ General settings tab, in the “Linked Frameworks and Libraries” section, drag and drop each framework you want to use from the Carthage/Build folder on disk.

On your application targets’ Build Phases settings tab, click the + icon and choose New Run Script Phase. Create a Run Script in which you specify your shell (ex: /bin/sh), add the following contents to the script area below the shell:

```bash
/usr/local/bin/carthage copy-frameworks
```

Create a file named ***APP_FOLDER/Carthage/InputFiles.xcfilelist*** and a file named ***APP_FOLDER/Carthage/OutputFiles.xcfilelist***

Add the paths to the frameworks you want to use to your ***InputFiles.xcfilelist***. For example:

```bash
$(SRCROOT)/Carthage/Build/iOS/IQKeyboardManagerSwift.framework
```

Add the paths to the copied frameworks to the ***OutputFiles.xcfilelist***. For example:

```bash
$(BUILT_PRODUCTS_DIR)/$(FRAMEWORKS_FOLDER_PATH)/IQKeyboardManagerSwift.framework
```


With output files specified alongside the input files, Xcode only needs to run the script when the input files have changed or the output files are missing. This means dirty builds will be faster when you haven't rebuilt frameworks with Carthage.

Add the ***InputFiles.xcfilelist*** to the "Input File Lists" section of the Carthage run script phase

Add the ***OutputFiles.xcfilelist*** to the "Output File Lists" section of the Carthage run script phase

## Step 7: Jazzy Setup

Add, inside ***APP_FOLDER***:

```bash
curl -O https://raw.githubusercontent.com/inacioferrarini/step-by-step/master/resources/.jazzy.yml
```

## Step 8: Swiftlint

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

## Step 9: Folder Structure for Modules

Create ***MODULES_FOLDER*** inside ***REPOSITORY_ROOT_FOLDER***, where all modules will be located.

Create ***TEMP_FOLDER*** inside ***REPOSITORY_ROOT_FOLDER***, where all temporary modules will be located.

## Step 10: Creating a Module

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

Finally, add the new module to the main App.

Inside ***APP_FOLDER***, add a new line on ***Cartfile***, with the new module:

```bash
github "submodule-repository-url" "branch"
```

Add the new framework as any other Carthage dependencies.