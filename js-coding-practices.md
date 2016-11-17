# Writing JavaScript code for the MUSIT project

## Coding Practices

* When committing code, let the precommit checks execute
  - Fix eventual errors
  - run ```git add .```
  - Commit again
* Each new file should have at least one test accompanied in the same directory under the folder ```__tests__```.
  - The test files should be named ```myModule-test.js```
  - The tests should cover as much as possible of the branches in the module to test.
  - The tests should not be made for test coverage only, but to test important functionality of the module in question.
