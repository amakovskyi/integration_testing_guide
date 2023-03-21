# Environment setup

### Use Docker for everything.

### Prepare ```Dockerfile``` for each service in the project:

* API services
* databases
* other

### Use mocked external services, where possible:

* ```AWS``` can be replaced by ```Localstack```
* Email sending service can be replaced by ```FakeSMTP``` or ```MailHog```

### Prepare ```tests``` project:

* Better to NodeJS platform no matter which main development technology for your project.
* Create a class which simulates your API client: login, network requests, authorization, handling refresh tokens, etc.
* Prepare direct access to your database from the "tests" project. Sometimes it is necessary to do tricks with data to
  simulate complex flows. Use raw queries, do not use any ORM mapping.
* Prepare direct access to external services, which cannot be mocked: Stripe etc.

### Setup Jest

Use ```jest``` and adjust the next settings in ```package.json```, if needed:

```json
{
  "scripts": {
    "test": "jest --runInBand"
  },
  "jest": {
    "rootDir": "test",
    "testRegex": ".*\\.test\\.ts$"
  }
}
```

* ```jest --runInBand``` is needed because the tests are running on a real environment and cannot be isolated. Parallel
  runs of some tests can cause them to fail.