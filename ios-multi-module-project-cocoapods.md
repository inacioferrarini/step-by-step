# About

This guide provides a step-by-step approach in order to create a modular iOS App, convering from creating the app itself to creating modules and adding these modules to the app.

To share the artifacts, *CocoaPods* will be used.

Convering from creating the app itself to creating modules and adding these modules to the app.

# Conventions

* ***GIT_URL***: URL for git repository.
* ***REPOSITORY_ROOT_FOLDER***: Checkout path for ***GIT_URL***.
* ***APP_FOLDER***: The folder where the App project is. Will be created inside ***REPOSITORY_ROOT_FOLDER***.
* ***MODULES_FOLDER***: Path for the module itself. Will be created inside ***REPOSITORY_ROOT_FOLDER***. In this guide, will be named `modules`
* ***TEMP_FOLDER***: Path for the temporary folder, where temporary artifacts will be created. Will be created inside ***REPOSITORY_ROOT_FOLDER***. In this guide, will be named `temp`.
* ***PRIVATE_REPO_NAME***: Name for private cocoapods' PodSpecs repository

## Example:

* ***GIT_URL***: git@github.com:inacioferrarini/pipoca.git
* ***REPOSITORY_ROOT_FOLDER***: ~/Developer/iOS/pipoca
* ***APP_FOLDER***: ~/Developer/iOS/pipoca/pipoca-app
* ***MODULES_FOLDER***: ~/Developer/iOS/pipoca/modules
* ***TEMP_FOLDER***: ~/Developer/iOS/pipoca/temp
* ***PRIVATE_REPO_NAME***: PRIVATE_SPECS

# Requirements

[Cocoapods](cocoapods-setup.md) - External dependencies and modules management.

# Project Setup

## Step 1: Git Repository Setup

Create a repository to host it. Follow the instructions from the project host for its initial setup.

> **Notes:**
> 1. It is handy to init the repository with a README.MD file. This guide will add required files.
> 1. The default branch will be *master*. Is recommended to change it to *develop*.

## Step 2: Creating the Project

Create the project on Xcode, inside ***REPOSITORY_ROOT_FOLDER***. The project location will be refered as ***APP_FOLDER***.

## Step 3: Gitignore Setup

Add, inside ***REPOSITORY_ROOT_FOLDER***, a file named ***.gitignore***, with the content:

```bash
### Swift ###
# Xcode

## Build generated
build/
DerivedData/

## Various settings
*.pbxuser
!default.pbxuser
*.mode1v3
!default.mode1v3
*.mode2v3
!default.mode2v3
*.perspectivev3
!default.perspectivev3
xcuserdata/

## Other
*.moved-aside
*.xccheckout
*.xcscmblueprint

## Obj-C/Swift specific
*.hmap
*.ipa
*.dSYM.zip
*.dSYM

.build/

# CocoaPods
Pods/

# Carthage
# Carthage/Checkouts
# Carthage/Build

# fastlane
fastlane/report.xml
fastlane/Preview.html
fastlane/screenshots/**/*.png
fastlane/test_output
fastlane/slather_report

### Xcode Patch ###
*.xcodeproj/*
!*.xcodeproj/project.pbxproj
!*.xcodeproj/xcshareddata/
!*.xcworkspace/contents.xcworkspacedata
/*.gcno

### Idea (for Appcode users) ###
.idea/

.DS_Store

### ixguard ###

ixguard-license.txt
ixguard.log
mapping.yml
statistics.yml

### slather ###

html/

### EnsureIT ###

EnsureIT/

### Temporary folder ###

temp/

```

or download it inside ***REPOSITORY_ROOT_FOLDER***:

```bash
curl -O https://raw.githubusercontent.com/inacioferrarini/step-by-step/master/resources/.gitignore
```

## Step 4: Gemfile Setup

Add, inside ***REPOSITORY_ROOT_FOLDER***, a file named ***Gemfile***, with the content:

```bash
source 'https://rubygems.org'

gem 'fastlane', '~> 2.120.0'
gem 'cocoapods', '~> 1.7.4'
gem 'slather', '~> 2.4.7'
```

or download it inside ***REPOSITORY_ROOT_FOLDER***:

```bash
curl -O https://raw.githubusercontent.com/inacioferrarini/step-by-step/master/resources/Gemfile
```

## Step 5: Ruby Version Setup

Add, inside ***REPOSITORY_ROOT_FOLDER***, a file named ***.ruby-version***, with the content:
```bash
2.5.1
```

or download it inside ***REPOSITORY_ROOT_FOLDER***:

```bash
curl -O https://raw.githubusercontent.com/inacioferrarini/step-by-step/master/resources/.ruby-version
```

## Step 6: Jazzy Setup

Add, inside ***REPOSITORY_ROOT_FOLDER***, a file named ***.jazzy.yml***, with the content:
```yaml
author: [YOUR NAME]
author_url: [TWITTER_URL]
github_url: [GIT_URL]
root_url: [DOCUMENTATION_REPOSITORY_URL]
module: [PROJECT_NAME]
output: docs
theme: fullwidth
xcodebuild_arguments: [-workspace, '[PROJECT].xcworkspace', -scheme, '[PROJECT_SCHEME]']
```

or download it inside ***REPOSITORY_ROOT_FOLDER***:

```bash
curl -O https://raw.githubusercontent.com/inacioferrarini/step-by-step/master/resources/.jazzy.yml
```

## Step 7: Initialize CocoaPods

Execute, inside ***APP_FOLDER***:

```bash
bundle exec pod init
```

## Step 8: Folder Structure for Modules

Create ***MODULES_FOLDER*** inside ***APP_FOLDER***, where all modules will be located.

Create ***TEMP_FOLDER*** inside ***APP_FOLDER***, where all temporary modules will be located.

## Step 9: Creating a Module

A module is used to keep features organized and grouped together, helps to avoid code duplication and allows to isolate features and provide a public-acces entry point to a feature.

Each module will have its own git repository.

### Creating a New Module

```bash
# Inside TEMP folder, to create a module named [MODULE_NAME] execute:
1. bundle exec pod lib create [MODULE_NAME]

# Some questions will be asked:
# platform: iOS
# platform: Swift
# demo app: no
# testing frameworks: Quick
# View based testing: Yes
```

### Updating Default Pod Lib configuration

Inside ***[MODULE_NAME]/[MODULE_NAME]***, create a folder named ***Sources*** and delete ***Classes*** folder.

Create a folder named ***[MODULE_NAME]*** inside ***Sources***.

### Update podspec file

Update ***MODULE_NAME/MODULE_NAME.podspec***
```ruby
  s.default_subspec = "[MODULE_NAME]"
  s.subspec "[MODULE_NAME]" do |ss|
    ss.source_files  = "[MODULE_NAME]/Sources/[MODULE_NAME]/**/*.swift"
    ss.resources = ["[MODULE_NAME]/Sources/[MODULE_NAME]/**/*.storyboard"]
    ss.framework  = "Foundation"
 end
```

For modules used by other modules, it is required to push the podspec to the private repository
```bash
$ git tag 'v0.0.2'
$ git push --tags
$ bundle exec pod repo update [PRIVATE_REPO_NAME]
$ bundle exec pod repo push [PRIVATE_REPO_NAME] module_name.podspec
```

Go to ***MODULES_FOLDER***

```bash
$ git submodule add [GIT_MODULE_URL] [MODULE_NAME]
```

Finally, add the new module to the main App.

Edit ***APP_FOLDER/Podfile***

Add the new dependency

```ruby
pod 'module-name', :path => '../modules/module-path'
```

Execute Cocoapods

```bash
$ bundle exec pod install
```
