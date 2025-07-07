# Are The Types Wrong checker

This action runs and validates npm packages with [Are The Types Wrong (attw)](https://arethetypeswrong.github.io/).
It will run `attw --pack <directory> --ignore-rules 'cjs-resolves-to-esm'` under the hood to pack the npm package, validate it, then clean up afterwards.

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
      - name: Clone the repository
        uses: actions/checkout@v4

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
      - name: Clone the repository
        uses: actions/checkout@v4

      - name: Check and validate ${{ matrix.project }}'s package.json
        uses: boyum/attw-action@v1
        with:
          working-directory: ${{ matrix.project }}
```

#### Only allow CommonJS packages

To fail if the package.json contains `"type": "module"`, use the `block-esm` option.
This will make sure your module can be imported to CommonJS projects without the use of dynamic `import()`.

```yml
name: Check and validate package.json

on:
  - pull_request

jobs:
  check-package:
    runs-on: ubuntu-latest

    steps:
      - name: Clone the repository
        uses: actions/checkout@v4

      - name: Check and validate package.json
        uses: boyum/attw-action@v1
        with:
          block-esm: true
```

### Options

| Name                | Required | Default value | Description                                                                                                                    |
| ------------------- | -------- | ------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `working-directory` | false    | `.`           | The directory where package.json is located, relative to the Git project.                                                      |
| `block-esm`         | false    | `false`       | Fail if the package.json contains `"type": "module"`. This turns off `--ignore-rules 'cjs-resolves-to-esm'` behind the scenes. |

## Development

### Releasing

To release a new version, create a new release in GitHub's Releases tab.
Also, overwrite the main version tag (currently v1) to point to the new release:

```bash
git tag -f v1
git push -f origin v1
```
