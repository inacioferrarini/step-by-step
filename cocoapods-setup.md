# About

For information about `CocoaPods`, what it is, etc, check [CocoaPods](https://cocoapods.org/)

# Requirements

`CocoaPods` requires `Ruby` and its environment. Check [Check Ruby / rbenv environment setup](ruby-rbenv-setup.md)

# Installing CocoaPods

To install CocoaPods:
```bash
gem install cocoapods
```

# Cocoapods Private Podspec Repository

To share artifacts in anonymous way, create a private `git` repository in order to host the `Podspec` repository.

The repository name will be refered as `[REPO_NAME]`.
This repository will be refered as `[SOURCE_URL]`.

Initialize the `git` repository

```bash
echo "# Private Podspecs" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin [SOURCE_URL]
git push -u origin master
```

Add the new `PodSpec` repository to `CocoaPods` installation.

```bash
pod repo add [REPO_NAME] [SOURCE_URL]
cd ~/.cocoapods/repos/[REPO_NAME]
pod repo lint .
```

