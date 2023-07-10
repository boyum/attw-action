# Are The Types Wrong checker

This action runs and validates npm packages with [Are The Types Wrong (attw)](https://arethetypeswrong.github.io/).
It will run `attw --pack .` under the hood to pack the npm package, validate it, then clean up afterwards.

## Usage

### Examples

#### Check current project in a single repo

```yml
name: Check and validate package.json

on:
  - pull_request

jobs:
  check-package:
    runs-on: ubuntu-latest

    steps:
      - name: Check and validate package.json
        uses: boyum/attw-action@v1
```

#### Check multiple projects in a monorepo

```yml
name: Check and validate package.json

on:
  - pull_request

jobs:
  check-package:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        project:
          - projects/a
          - b
          - projects/c/d

    steps:
      - name: Check and validate ${{ matrix.project }}'s package.json
        uses: boyum/attw-action@v1
        with:
          working-directory: ${{ matrix.project }}
```

### Options

| Name                | Required | Default value | Description                                                               |
| ------------------- | -------- | ------------- | ------------------------------------------------------------------------- |
| `working-directory` | false    | `.`           | The directory where package.json is located, relative to the Git project. |
