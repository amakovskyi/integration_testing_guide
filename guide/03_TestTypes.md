# Integration test types

### Functional

This type of test should validate every single endpoint works correctly.

* Create a test file per endpoint according to folder structure end naming.
* Measure an endpoint.
* Create a test for each single measured dimension.
* When an endpoint should have prepared to be called (register user, create other entities), do it using other public
  API
  endpoints:
    * If possible - in ```beforeAll()``` section.
    * If not possible - per test.
    * If several, but not all, tests can use the same setup - divide them into subsections with their
      own ```beforeAll()```
    * Big and complex tests can be separated by files:
        * ```POST.doSome.test``` - for general validation
        * ```POST.doSome.SpecificValidation.test``` - for complex validation of specific functionality

### Interoperation

These tests cover features when the call of one endpoint can indirectly affect the behaviour of another endpoint.

Examples: usage of an admin-related endpoint to block user cause:

* **FUNCTIONAL** test: direct flag change for a user in admin endpoint "getUsers" to ```"blocked": true```.
* **INTEROPERATION** test: indirect change of look of how this user displayed in chat members list: for example,
  erased name and profile photo. As this specific behaviour can affect an endpoint "far" from which the initial action
  is done,
  it should not litter functional tests.
  In this case, there are two feature roles:
* _Initiator_: a feature which causes interoperation. In the example above it is a "block user" feature.
* _Responder_: a feature which changes its behaviour because of interoperation. In the example above, it is an "erased
  name and
  profile photo" feature. In many cases, there are many responder features. For example, it can affect user profiles,
  user
  comments under posts, user chat messages etc.

Flow:

* Setup test by using the _initiator_ feature in ```beforeAll()```
* Write down tests for each _responder_ feature

### Security

Each application security can have different logic, but commonly security patterns around a single controller are the
same:

* Validate only users with permissions can invoke the endpoint:
    * by user permissions
    * by user roles
    * by entity ownership (for example, in a social network, only post author can edit it)
* Validate is the endpoint correctly handles calls by non-authorized users (works correctly if responds with 401)
* Other, if applicable.

### Validation

This validation should cover situations when an incoming request is wrong (```400: Bad Request```).

* Prepare endpoint for usage, like for **functional** tests.
* For every single parameter or field, try to pass incorrect values:
    * wrong type (_string_ instead of _int_, _boolean_ instead of _UUID_, etc.)
    * wrong value for enums
    * pass null when it is not supported
    * do not pass a value at all
    * if a value is ID, pass not-existing ID (random _UUID_, negative _int_ etc.)
    * pass an empty string for just string values
    * pass values out of bounds for numbers
    * etc.
* Validate API responds with the correct response code and message describing that wrong data passed.

In general, it is better to implement all incoming requests validation and do not pass a request into services for
processing
if it is not completely valid.

1. When the server responds with any ```500: Internal Server Error```, then the request is partially processed. In most
   cases, database transactions can save you from broken data in the database, but better not to process the initially
   wrong
   request at all.
2. Input validation allows client-side developers (frontend or integrators) easily understand their
   errors and integrate API much faster without asking back-end developers to help and interrupting their work.

### Feature

Feature tests work not around a single endpoint or controller but on specific features.

It is like testing user stories. Simulate feature flows as expected to be used by the end user. Do the same as your
testers are going to do :)

By the fact mostly these tests are not needed, as other types of tests cover everything. But when it is required -
you will "feel" it.

### Anything else

If any tests are needed but not under these categories, you can place them in the appropriate folder based on the
specific application structure.
