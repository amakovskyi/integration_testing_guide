## The guide on how to perform integration testing of back-end applications

Integration tests, when written in the right way, can:

* improve project development and delivery performance
* vastly increase product quality

And they are doing it simultaneously.

This guide contains the most common things about integration tests.

-------

* Integration tests use public API, while unit tests use internal code interfaces. Because of that, unit tests require
  fixing when the interface changes, but integration tests do not until the public API does not introduce breaking
  functional changes. Because of that, most integration tests can be without changes during the project's lifetime.
  Forget about "I am fixing tests."
* Unit tests verify behaviour in isolated conditions, where code will never be on production. Integration tests
  verify how code works in real conditions and verify all dependent services: databases, mail, integration
  with external services like AWS S3 etc.
* Developers always test their code with tools like Postman. Integration tests are just the same but already
  automated and reusable. And they are executing much faster than inside Postman.

_______

**CONTENTS**

[1. Environment setup](guide/01_EnvironmentSetup.md)

[2. Files and folders structure](guide/02_FileAndFolderStructure.md)

[3. Integration test types](guide/03_TestTypes.md)

[4. Using API commons](guide/04_Commons.md)

[5. Test cascading](guide/05_Cascading.md)

[6. Tips and tricks](guide/06_TipsAndTricks.md)

[7. Useful advice](guide/07_UsefulAdvice.md)

_______

# api-auditor

A NodeJS library for integration testing.

[https://www.npmjs.com/package/@amakovskyi/api-auditor](https://www.npmjs.com/package/@amakovskyi/api-auditor)

