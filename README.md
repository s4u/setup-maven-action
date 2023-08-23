# Setup Maven Action
[![Test](https://github.com/s4u/setup-maven-action/actions/workflows/test.yml/badge.svg)](https://github.com/s4u/setup-maven-action/actions/workflows/test.yml)

This is composite action which help to prepare GitHub Actions environment for Maven build by calling:

- [actions/checkout](https://github.com/marketplace/actions/checkout)
- [actions/setup-java](https://github.com/marketplace/actions/setup-java-jdk)
- [actions/cache](https://github.com/marketplace/actions/cache)
- [stCarolas/setup-maven](https://github.com/marketplace/actions/setup-maven)
- [s4u/maven-settings-action](https://github.com/marketplace/actions/maven-settings-action)

:exclamation: You **should not** include above actions in your configuration - in other case  those will be **called twice**. :exclamation:

For default values you only need:

```yml
    steps:

      - name: Setup Maven Action
        uses: s4u/setup-maven-action@< version >

      - run: mvn -V ...
```
 
# Params mapping for sub actions

## checkout

| params                       | destination         | default |
|------------------------------|---------------------|---------|
| checkout-fetch-depth         | fetch-depth         |         |
| checkout-path                | path                |         |
| checkout-ref                 | ref                 |         |
| checkout-persist-credentials | persist-credentials | false   |

## setup-java

| params            | destination  | default |
|-------------------|--------------|---------|
| java-version      | java-version |         |
| java-distribution | distribution | temurin |

## cache

A cache action is configured as:

```yaml
    - uses: actions/cache
      with:
        path: |
          ${{ inputs.cache-path }}
          ${{ inputs.cache-path-add }}
        key: ${{ inputs.cache-prefix }}${{ runner.os }}-jdk${{ inputs.java-version }}-${{ inputs.java-distribution }}-maven${{ inputs.maven-version }}-${{ hashFiles('**/pom.xml') }}
        restore-keys: ${{ inputs.cache-prefix }}${{ runner.os }}-jdk${{ inputs.java-version }}-${{ inputs.java-distribution }}-maven${{ inputs.maven-version }}-
```

So we can use for action:

| params         | description                                              |
|----------------|----------------------------------------------------------|
| cache-path     | default cache path for Maven with value ~/.m2/repository | 
| cache-path-add | additional value for cache path                          |
| cache-prefix   | prefix value for `key` and `restore-keys` cache params   |


## setup-maven

| params        | destination   | default |
|---------------|---------------|---------|
| maven-version | maven-version | 3.9.4   |

## maven-settings-action

| params                     | destination       |
|----------------------------|-------------------|
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
        maven: [ '3.8.8', '3.9.4' ]

    name: Maven ${{ matrix.maven }} sample

    steps:

      - name: Setup Maven Action
        uses: s4u/setup-maven-action@< version >
        with:
          java-version: 8
          maven-version: ${{ matrix.maven }}

      - run: mvn -V ...
```

# Contributions

- Contributions are welcome!
- Give :star: - if you want to encourage me to work on a project
- Don't hesitate to create issues for new features you dream of or if you suspect some bug

# Project versioning

This project uses [Semantic Versioning](https://semver.org/).
We recommended using the latest and specific release version.

In order to keep your project dependencies up to date you can watch this repository *(Releases only)*
or use automatic tools like [Dependabot](https://docs.github.com/en/code-security/supply-chain-security/keeping-your-dependencies-updated-automatically/about-dependabot-version-updates).

# License

The scripts and documentation in this project are released under the [MIT License](LICENSE)
