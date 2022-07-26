# Using API commons

Integration test requires many API invocations for test setup:

* creating users
* making payments
* creating required entities
* etc.

When functional or validation testing is performed for endpoint - it is right to compose whole request directly in test,
as it purely validates behavior of this specific endpoint.

But after it ss tested it can be used many times in other tests. Fulling full requests every time can be too time and
code-lines expensive, as also sometimes APIs are updated or extended.

So, instead of writing tons of manually composed requests well-tested API should be organized into **commons** files.

```
test/
  main_api/
    path/
      to/
        controller/
          _functional
          _security
          _validation
          subpath/
          PathToController.commons.ts
```

After API is tested just create file named ```PathToController.commons.ts``` with content:

```typescript
export class PathToControllerCommons {

    static async enpoint1Call(user: ApiClient, params: {}) {
        // IMPLEMENTATION
    }

    static async enpoint2Call(user: ApiClient, params: {}) {
        // IMPLEMENTATION
    }

    static async enpoint3Call(user: ApiClient, params: {}) {
        // IMPLEMENTATION
    }

    static async complexCreationFlow(user: ApiClient, params: {}) {
        // IMPLEMENTATION
    }

}
```

Here it is possible to implement request filling with all default data, except required by ```params```, of perform
complex flow like "create - publish", etc to single-line invocation from other tests.

Outside tests for exactly this controller use commons in all places where it is possible. Even if there are only single
invocation, as after short time for sure it will be needed again.
