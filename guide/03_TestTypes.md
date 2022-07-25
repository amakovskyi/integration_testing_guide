# Integration test types

### Feature tests

Feature tests works not around single endpoint or controllers, but on specific features.

Simulate feature flows as it expected to be used by end user.

### Functional

This types of tests should validate each single endpoint works correctly.

* Create test file per endpoint according to folder structure end naming
* Measure endpoint
* Create test for each single measured dimension
* When endpoint should have preparation to be called (register user, create other entities) do it using other public API
  endpoints:
    * If possible - in ```beforeAll()``` section
    * If not possible - per test
    * If several, but not all, tests can use same setup - divide them on subsections with own ```beforeAll()```
    * Big and complex tests can be separated by files:
        * ```POST.doSome.test``` - for general validation
        * ```POST.doSome.SpecificValidation.test``` - for complex validation of specific functionality

### Interoperation

These tests cover features, when call of one endpoint can indirectly affect behavior of other endpoint.

Examples: usage of admin-related endpoint to block user cause:

* **FUNCTIONAL** test: direct change of flag for user in admin endpoint "getUsers" to ```"blocked": true```.
* **INTEROPERATION** test: indirect change of look of how this user displayed in chat members list: for example,
  erased name and profile photo. As this specific behavior can affect endpoint "far" from which initial action done
  it should not litter functional tests.
  In this case there are two feature-roles:
* _Initiator_: feature which causes interoperation. In example above it is "block user" feature.
* _Responder_: feature, which changes its behavior because of interoperation. In example above it is "erased name and
  profile photo" feature. In many cases there are many responder features, for example it can affect user profile, user
  comments under posts, user chat messages etc.

Flow:

* Setup test by using _initiator_ feature in ```beforeAll()```
* Write down test for each _responder_ feature

### Security

Each application security can have different logic, but commonly security patterns around single controller are same:

* Validate only user with permissions can invoke endpoint:
    * by user permissions
    * by user roles
    * by entity ownership (for example, in social network only post author can edit it)
* Validate is endpoint correctly handles calls by non-authorized users (works correctly of responds with 401)
* Other, if applicable.

### Validation

This validation should cover situations when incoming request is wrong (```400: Bad Request```).

* Prepare endpoint for usage like for **functional** tests.
* For each single parameter/field try to pass incorrect values:
    * wrong type (_string_ instead of _int_, _boolean_ instead of _UUID_, etc.)
    * wrong value for enums
    * pass null when it is not supported
    * do not pass value at all
    * if value is ID, pass not-existing ID (random _UUID_, negative _int_ etc.)
    * pass empty string for just string values
    * pass values out of bounds for numbers
    * etc.
* Validate API responds with correct response code and message with description of wrong data passed.

In general, it is better to implement all incoming requests validation and not pass request into services for processing
if it is not completely valid.

1. When server responds with any ```500: Internal Server Error``` it means that request was partially processed. In most
   cases transactions can save you from broken data in the database, but better to do not process initially wrong
   request at all.
2. Input validation allows client-side developers (frontend or integrators) easily understand their
   errors and integrate API much faster, without asking back-end developers to help and interrupting their work.

### Feature

It is like testing users stories. Do same what your testers going to do :)

By the fact mostly these tests not needed, as other types of tests covers everything. But in cases when it is needed -
you will "feel" it.

### Anything else

If there are any tests, which are needed but not under any of these categories - you can place them in appropriate
folder, based on specific application structure.