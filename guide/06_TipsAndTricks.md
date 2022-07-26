# Tips and tricks

Sometimes public API is not enough to validate some complex features and flows.

For such cases there are set of tricks which can be used for integration tests.

### Service endpoints

Sometimes for performing integration tests it is needed to have endpoints, which are not needed by users.

_For example, when you test list content, it is good to have well-tested endpoint which returns list item by its id
instead of manual validation each list item. With such endpoint you can just compare content of list to content of list
item, returned by such endpoint._

_Another case is when it is needed to trigger immediately (for testing purposes) some function on back-end service which
normally triggers by some event (cron job, etc.)._

For such cases it is OK to create additional endpoint for this. If endpoint should be restricted for usage it can be
possible to make config which allows to invoke it only in test environment or restrict its usage only by specific users.

Also, in addition to service endpoint, it is possible to add additional data (needed for tests) to standard endpoints,
if it is not exposes any information, restricted to users.

### Service users

Service users is like service endpoints - not for public usage. But you can create test users with specific id and/or
permissions which allowed to execute some actions, generally forbidden to other users, and use them for testing.

Restriction of access on production environment can be implemented by:

* creating such user without login credentials on production (no email - cannot restore password, cannot log in at all)
* do not create such user at all on production
* do not put service permissions into production database (it will not allow to grant permission even if exists in code)

### "Illegal" manipulations

"Illegal" manipulations is when your test uses direct access to application internals: direct access to a database or
external APIs (AWS, Stripe etc.) which normally closed to client-side.

For some cases it is hard to prove correctness of how your application works just by using public API and service
users or service endpoints. For these cases it is **allowed** to access directly to closed parts of application.

BUT! Its usage should be restricted to only cases when there is no other way to do it (like using service endpoints and
users). As tests with usage of closed APIs dives into implementation details, these tests can be affected by coda
changes like unit tests, requiring fixes and support after code changes.

Taking into account mentioned above such tests can be very useful for testing some parts of your application.

### Mocking "expensive" flows (shortcuts)

Some testing flow can be very time-expensive, but it becomes a problem when such flow used widely in testing.

_For example, simulating payment checkout flow to validate how access works with paid subscription - means proceed
checkout with payment system in test mode using headless browser: it takes about 20 seconds - too much to use it in
dozens of tests._

Instead of that you can directly create subscription in your database or by admin API of your payment provider - it is
named **shortcut**. It allows to mock expensive flow by another flow, which produces same result in context of paid
feature testing, but it executes much faster. It is allowed and frequently required to use "illegal" manipulations,
service endpoint and users for creating shortcuts.

**But how to be sure that full flow and shortcut will work in same way?**

Let's add... integration test, which verifies, that both flows functions in the same way :)

Create test with next features:

* setup one entity (user) and proceed with full flow
* setup another entity (user) and proceed with shortcut flow
* validate both flows equally saved to a database
* for each feature, affected by this flow, test both entities (users) with single simple and validate it works in same
  expected way

After you done all above you can use shortcut flow for testing feature. This test proves it works in same way as
original expensive flow.
