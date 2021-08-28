# Setup Maven with settings.xml
[![Test](https://github.com/s4u/setup-maven-action/actions/workflows/test.yml/badge.svg)](https://github.com/s4u/setup-maven-action/actions/workflows/test.yml)

This is composite action which help to prepare GitHub Actions environment for Maven build by calling:

- [actions/setup-java](https://github.com/marketplace/actions/setup-java-jdk)
- [stCarolas/setup-maven](https://github.com/marketplace/actions/setup-maven)
- [s4u/maven-settings-action](https://github.com/marketplace/actions/maven-settings-action)

# Contributions
- Contributions are welcome!
- Give :star: - if you want to encourage me to work on a project
- Don't hesitate to create issues for new features you dream of or if you suspect some bug

# Project versioning
This project uses [Semantic Versioning](https://semver.org/).
We recommended to use the latest and specific release version.

In order to keep your project dependencies up to date you can watch this repository *(Releases only)*
or use automatic tools like [Dependabot](https://docs.github.com/en/code-security/supply-chain-security/keeping-your-dependencies-updated-automatically/about-dependabot-version-updates).

# Params mapping for sub actions

## setup-java

| params            | destination  | default |
| ----------------- |------------- |-------- |
| java-version      | java-version |         |
| java-distribution | distribution | temurin |
| java-cache        | cache        |         |

## setup-maven

| params        | destination   | default |
| ------------- |-------------- |-------- |
| maven-version | maven-version | 3.8.1   |

## maven-settings-action

| params                     | destination       |
| -------------------------- |------------------ |
| settings-servers           | servers           |
| settings-mirrors           | mirrors           |
| settings-properties        | properties        |
| settings-sonatypeSnapshots | sonatypeSnapshots |

# Testing against different Maven versions

```yaml

jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        maven: [ '3.5.4', '3.6.3', '3.8.2' ]

    name: Maven ${{ matrix.maven }} sample

    steps:
      - uses: actions/checkout@v2

      - name: Setup Maven
        uses: setup-maven@v1.0.0
        with:
          java-version: 8
          maven-version: ${{ matrix.maven }}
      - run: mvn -V ...
```

# License

The scripts and documentation in this project are released under the [MIT License](LICENSE)
