# Test cascading

Cascading is a technique when one endpoint is used to validate correctness of another endpoint. Generally, all
integration test based on cascading, but for specific cases cascading is required to be developed specifically.

### Example 1. List testing

```GET /users?count&offset```

When it needed to test list of users it needed to validate at least these dimensions:

* "count" parameter
* "offset" parameter
* list items content

For dimension "list items content" it needed to take each single item and validate its data.
Instead of validating each single item using data from public profile, etc. it needed to:

* create new endpoint: ```GET /users/{id}``` which should return exactly same model as used in list item.
* write full integration test coverage for endpoint ```GET /users/{id}``` (with all possible data variations)
* fill users list with all possible data variations
* retrieve list by ```GET /users``` and validate each item from list is it strictly equal to item, returned
  from ```GET /users/{id}```

Instead of manual validation each single user in list we used endpoint  ```GET /users/{id}``` for performing cascade
validation.

### Example 2. Inner objects

```GET /userFeed```

This endpoint returns list of user posts with full data about their ```author```:

```json
{
  "id": "postId",
  "test": "string",
  "author": {}
}
```

It is needed to validate that ```author``` field returned right, but instead of validation this data manually let's use
cascading technique:

* create endpoint ```GET /users/authorProfile/{id}```
* write full integration test coverage for endpoint ```GET /users/authorProfile/{id}``` (with all possible data
  variations)
* fill posts list by authors with all possible user data variations
* retrieve data from ```GET /users/authorProfile/{id}``` for each post by author id
* validate is it strictly equal to ```author```

Instead of manual validation each single author profile in posts list we used
endpoint ```GET /users/authorProfile/{id}``` for performing cascade
validation.
