# About

This guide provides a step-by-step approach in order to create the `Ruby` environment.

# Installing Ruby and rbenv

## Installing `Homebrew`

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## Installing `rbenv` and `Ruby`

```bash
brew install rbenv ruby-build

echo 'if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi' >> ~/.bash_profile
source ~/.bash_profile

rbenv install 2.6.3
rbenv global 2.6.3
ruby -v
```