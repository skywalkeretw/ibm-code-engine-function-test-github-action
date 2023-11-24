# Code Engine Function Test GitHub Action

This GitHub Action allows you to test IBM Cloud Code Engine functions. It provides flexibility based on the programming language used in your function and supports various configuration options.

## Author
- Author: Skywalker_etw

## Description
This action is designed for testing IBM Cloud Code Engine functions. It allows you to specify the programming language, source code directory, and the file to run for testing.

### Inputs

| Name          | Required | Default Value | Description                                                  |
|---------------|----------|---------------|--------------------------------------------------------------|
| `runtime`     | ✅        | -             | Programming language used by your function.  (`nodejs-18`, `python-3.11`)                |
| `source-dir`  | ❌        | .             | Path to the directory containing the source code.           |
| `test-file`   | ❌        | `*test*.py` / `*test*.js` | Specify the file that should be run to test your function. If not specified, a file containing the name `test` will be run based on the runtime.|

## Usage

To use this action, add it to your GitHub Actions workflow YAML file. For example:

```yaml
name: Test Code Engine Function

on:
  push:
    branches:
      - main
      - master

jobs:
  test-function:
    runs-on: ubuntu-latest
    steps:
    - name: Check out code
      uses: actions/checkout@v3

    - name: Test IBM Cloud Code Engine Function
      uses: skywalkeretw/ibm-code-engine-function-test@v1
      with:
        runtime: 'nodejs-18'
        source-dir: './function'
        test-file: 'my-function-test.js'
```