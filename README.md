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

### Github action 
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
        source-dir: './js-function'
        test-file: 'main.test.js'
```

### Function code: `main.js`
```javascript
function main(params) {
    return {
        headers: { 'Content-Type': 'text/html; charset=utf-8' },
        body: `<html><body><h3>Hello World</h3></body></html>`
     }
}

module.exports.main = main;
```

### Function Test code: `main.test.js`
```javascript
const chai = require('chai');
const { main } = require('./main');
const expect = chai.expect;

describe('main function', () => {
  it('should return Hello World', () => {
    const params = {};
    const result = main(params);

    const expected = {
      headers: { 'Content-Type': 'text/html; charset=utf-8' },
      body: '<html><body><h3>Hello World</h3></body></html>'
    };

    expect(result).to.deep.equal(expected);
  });
});
```
---

### Github action 
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
        source-dir: './py-function'
        test-file: 'my-function-test.js'
```

### Function code: `__main__.py`
```python
def main(params):
    return {
        "headers": {
            'Content-Type': 'text/html; charset=utf-8',
        },
        "statusCode": 200,
        "body": f"<html><body><h3>Hello World</h3></body></html>",
    }
```

### Function Test code: `test_main.py`
```python
import unittest
from main__ import main

class TestMainFunction(unittest.TestCase):

    def test_without_name_param(self):
        params = {}
        result = main(params)

        expected = {
            "headers": {'Content-Type': 'text/html; charset=utf-8'},
            "statusCode": 200,
            "body": "<html><body><h3>Hello World</h3></body></html>",
        }

        self.assertEqual(result, expected)

if __name__ == '__main__':
    unittest.main()
```