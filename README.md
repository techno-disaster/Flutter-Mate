# Github Action for Flutter Testing

This action provides `flutter` for Github Actions.
Used on a large scale at [AnitaB-org/mentorship-flutter](https://github.com/anitab-org/mentorship-flutter/)
Builds can be found [here](https://github.com/anitab-org/mentorship-flutter/actions)

## Usage examples

This example first fetches the dependencies with `flutter pub get` and then
builds an apk and runs the flutter tests in parallel.

```
action.yml
name: 'Flutter Mate'
description: ' This action provides flutter for Github Actions.'
runs:
  using: 'docker'
  image: 'Dockerfile'
  ---------------------------------------
main.yml

name: Client build
on:
  push:
    paths:
      - '.github/workflows/**'
  pull_request:
    branches: [develop]
    paths:
      - '.github/workflows/**'
      - 'pubspec.yaml'
      - 'pubspec.lock'
      - 'lib/**'
      - 'ios/**'
      - 'android/**'
jobs:
  buildios:
    name: Build iOS
    runs-on: macos-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-java@v1
        with:
          java-version: '12.x'
      - run: git clone https://github.com/flutter/flutter.git --depth 1 -b v1.15.17 _flutter
      - run: echo "::add-path::$GITHUB_WORKSPACE/_flutter/bin"
      - run: flutter pub get
        working-directory: ./
      - run: flutter build ios --no-codesign
  buildandroid:
    name: Build Android
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-java@v1
        with:
          java-version: '12.x'
      - run: git clone https://github.com/flutter/flutter.git --depth 1 -b v1.15.17 _flutter
      - run: echo "::add-path::$GITHUB_WORKSPACE/_flutter/bin"
      - run: flutter pub get
        working-directory: ./
      - run: flutter build appbundle
```
