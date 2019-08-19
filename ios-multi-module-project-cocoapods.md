# About

This guide provides a step-by-step approach in order to create a modular iOS App.

To share the artifacts, `CocoaPods` will be used.

In order to check `CocoaPods` setup, check the guide here: [Cocoapods Setup](cocoapods-setup.md)

# Requirements

[Cocoapods](cocoapods-setup.md)

# Project Setup

## Step 1: Git Repository Setup

Do the Git setup for the project.

Create a repository to host it. The simplest solution is to create it on github. Follow the instructions from the project host for its initial setup.

It is handy to init the repository with a README.MD file. Other options, like license and .gitignore will be added in a future moment.

### Note:

`[GIT_URL]` will be used to reference the git url for the created repository.

`[PROJECT_ROOT]` will be used to reference the checkout path of the created repository, where the project will be created.

The default branch will be `master`. Is recommended to change to `develop`


## Step 2: Creating the Project

Create the project on Xcode, inside `[PROJECT_ROOT]`. The project will be refered as `[PROJECT]`.

## Step 3: Gitignore Setup

Add, inside `[PROJECT_ROOT]` folder, a file named `.gitignore`, with the content:

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

Temp/

```

or download it inside `[PROJECT_ROOT]` folder:

```bash
curl -O https://raw.githubusercontent.com/inacioferrarini/step-by-step/master/resources/.gitignore
```

## Step 4: Gemfile Setup

Add, inside `[PROJECT_ROOT]` folder, a file named `Gemfile`, with the content:

```bash
source 'https://rubygems.org'

gem 'fastlane', '~> 2.120.0'
gem 'cocoapods', '~> 1.7.4'
gem 'slather', '~> 2.4.7'
```

or download it inside `[PROJECT]` folder:

```bash
curl -O https://raw.githubusercontent.com/inacioferrarini/step-by-step/master/resources/Gemfile
```

## Step 5: Ruby Version Setup

Add, inside `[PROJECT]` folder, a file named `.ruby-version`, with the content:
```bash
2.5.1
```

or download it inside `[PROJECT]` folder:

```bash
curl -O https://raw.githubusercontent.com/inacioferrarini/step-by-step/master/resources/.ruby-version
```

## Step 6: Jazzy Setup

Add, inside `[PROJECT_ROOT]` folder, a file named `.jazzy.yml`, with the content:
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

or download it inside `[PROJECT_ROOT]` folder:

```bash
curl -O https://raw.githubusercontent.com/inacioferrarini/step-by-step/master/resources/.jazzy.yml
```

## Step 7: Folder Structure for Modules

Create a folder named `modules` inside `[PROJECT]`, where all modules will be located.

Create a folder named `Temp` inside `[PROJECT]`, where all modules will be placed upon creation, until being pushed to repository and fetched inside `modules`, and then removed from `Temp` folder.

## Step 8: Creating a Module

A module is used to keep features organized and grouped together, helps to avoid code duplication and allows to isolate features and provide a public-acces entry point to a feature.

Each module will have its own git repository.

### Creating a New Module

A temporary folder will be used, named `[PROJECT_ROOT]/Temp`

Once the module is created and pushed into `Git`, it will be added as a `git submodule` inside `[PROJECT_ROOT]/modules` folder.

```bash
# Inside TEMP folder, execute
1. bundle exec pod lib create [MODULE_NAME]

# Some questions will be asked:
# platform: iOS
# platform: Swift
# demo app: no
# testing frameworks: Quick
# View based testing: Yes
```

### Step 8.1: Updating Default Pod Lib configuration

Inside `module_name/module_name/Classes`, create a folder named `module_name`

### Step 8.2: Update `.podspec` file

Update `module_name/module_name.podspec`
```ruby
  s.default_subspec = "module_name"
  s.subspec "module_name" do |ss|
    ss.source_files  = "module_name/Classes/module_name/**/*.swift"
    ss.resources = ["module_name/Classes/module_name/**/*.storyboard"]
    ss.framework  = "Foundation"
 end
```

For modules used by other modules, it is required to push the podspec to the private repository
```bash
$ git tag 'v0.0.2'
$ git push --tags
$ bundle exec pod repo update [REPO_NAME]
$ bundle exec pod repo push [REPO_NAME] module_name.podspec
```

Go to `[PROJECT_ROOT]/modules` folder

```bash
$ git submodule add path folder
```

Finally, add the new module to the main App.

Edit `[PROJECT_ROOT]/[PROJECT]/Podfile`

Add the new dependency

```ruby
pod 'module-name', :path => '../modules/module-path'
```

Execute Cocoapods

```bash
$ bundle exec pod install
```