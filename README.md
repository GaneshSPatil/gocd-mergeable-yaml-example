## GoCD Mergable Yaml Example
![.github/workflows/gocd-mergable.yml](https://github.com/GaneshSPatil/gocd-mergable-yaml-example/workflows/.github/workflows/gocd-mergable.yml/badge.svg)

An example GoCD Yaml config repository which uses [Gocd Mergable](https://github.com/GaneshSPatil/gocd-mergable) github action.

### Description

This repository contains GoCD configurations specified in YAML format using [GoCD Yaml Config Plugin](https://github.com/tomzo/gocd-yaml-config-plugin). 
The GoCD Mergable github action verifies whether changes done to the GoCD configuration files are valid or not.

### Setup

1. Define GoCD Pipeline configurations in `dev.gocd.yaml` file

```yaml
format_version: 9
pipelines:
  build-on-linux:
    group: dev
    materials:
      mygit:
        git: http://example.com/mygit.git
    stages:
      - build:
          jobs:
            build:
              tasks:
                - exec:
                    command: make
environments:
  dev:
    environment_variables:
      DEPLOYMENT_ENV: dev
    pipelines:
      - build-on-linux
```

2. Create `.github/workflows/gocd-mergable.yml` to setup GoCD Mergable Github Action.

```yaml
on: [push, pull_request]

jobs:
  verify_config_repository:
    runs-on: ubuntu-latest
    name: verify config merge.
    steps:
      - name: Git checkout
        uses: actions/checkout@v2
      - name: Verify Config Merge
        uses: GaneshSPatil/gocd-mergable@v1.0.0
        with:
          GOCD_SERVER_URL: 'https://build.gocd.org/go'
          GOCD_ADMIN_ACCESS_TOKEN: ${{ secrets.GOCD_ADMIN_ACCESS_TOKEN }}
          GOCD_CONFIG_REPOSITORY_ID: 'plugin-api.go.cd-pipelines-yaml'
```

### License

GoCD Mergable Yaml Example is an open source project, under the [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0).

