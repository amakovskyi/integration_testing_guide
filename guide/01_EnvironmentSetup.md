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
* Create class, which simulates your API client: login, network requests, authorization, handling refresh tokens, etc.
* Prepare direct access to your database from tests project. Use raw queries, do not use any ORM mapping.
* Prepare direct access to external services, which cannot be mocked: Stripe etc.

### Setup Jest

Use ```jest``` and adjust next settings in ```package.json```, if needed:

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

* ```jest --runInBand``` is needed because of test are running on real environment and cannot be isolated one from
  another. Parallel run of some test can cause some tests to fail.