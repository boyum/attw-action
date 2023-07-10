# Are The Types Wrong checker

This action runs and validates npm packages with [Are The Types Wrong (attw)](https://arethetypeswrong.github.io/).
It will run `attw --pack .` under the hood to pack the npm package, validate it, then clean up afterwards.

## Usage

```yml
name: Check and validate package.json

on:
  - pull_request

jobs:
  check-package:
    runs-on: ubuntu-latest

    steps:
      - name: Clone the repository
        uses: actions/checkout@v3

      - name: Check and validate package.json
        uses: boyum/attw-action@v1
        with:
          working-directory: . # This is the default working directory and can be omitted
```

### Options

| Name | Required | Default value | Description |
|---|---|---|---|
| `working-directory` | false  | `.` | The directory where package.json is located, relative to the Git project. |
