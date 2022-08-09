## The guide how to perform integration testing of back-end applications

Integration tests, when written in the right way, can:

* improve project development and delivery performance
* vastly increase product quality

And they are doing it simultaneously.

This guide contains most common things about integration tests.

-------

* Integration tests uses public API, when unit tests uses internal code interfaces. Because of that unit tests require
  fixing when interface changes, but integration tests does not until public API does not introduce breaking
  functional changes. Because of that most of an integration tests can be without changes during project lifetime.
  Forget about "I am fixing tests".
* Unit tests verifies behavior in isolated conditions, in which code will never be on production. Integration tests
  verifies how code works in real conditions, in addition verifies all dependent services: databases, mail, integration
  with external services like AWS S3 etc.
* Developers always tests their code with tools like Postman. Integration tests is just the same, but already
  automated and reusable. And they are executing much faster than inside Postman.

_______

**CONTENTS**

[1. Environment setup](guide/01_EnvironmentSetup.md)

[2. Files and folders structure](guide/02_FileAndFolderStructure.md)

[3. Integration test types](guide/03_TestTypes.md)

[4. Using API commons](guide/04_Commons.md)

[5. Test cascading](guide/05_Cascading.md)

[6. Tips and tricks](guide/06_TipsAndTricks.md)

[7. Useful advices](guide/07_UsefulAdvices.md)

_______

# api-auditor

A NodeJS library for integration testing.

[https://www.npmjs.com/package/@amakovskyi/api-auditor](https://www.npmjs.com/package/@amakovskyi/api-auditor)

