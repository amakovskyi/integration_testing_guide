# Files and folders structure

### Naming

Use ```TestName.test.ts``` as base test name.

### Base folder structure

```
src/
test/
  _features/
  main_api/
  second_api/
package.json
```

* ```src``` contains classes, used for tests, but not test itself, like API client, utils etc.
* ```test/_features``` feature tests; use exactly this folder name;
* ```test/main_api``` tests for "main api"
* ```test/second_api``` tests for "second api"

Underscore character (```_```) is used to:

* identify standard folders
* place them on top of alphabetical sorting

### Feature tests

Folder ```test/_features``` should contain feature testing.

It is complex flows testing which can involve usage a lot of different endpoints from many services.

### API tests

Each api should have its own folder for tests. For example, if your application contains two APIs names "main" and "
second" use folders ```test/main_api``` and ```test/second_api```.

**Internal structure**

```
test/
  main_api/
    _interoperation
      FeatureInteroperatesWithOtherFeature.test.ts
    path/
      to/
        controller/
          _functional
            GET.endpoint1Name.test.ts
            POST.endpoint2Name.test.ts
            PUT.endpoint3Name.test.ts
          _security
            Roles.test.ts
            Other.test.ts
          _validation
            GET.endpoint1Name.test.ts
            POST.endpoint2Name.test.ts
            PUT.endpoint3Name.test.ts
          subpath/
            _functional
              ...
            _security
              ...
            _validation
              ...
```

* When ```/path/to/controller``` is controller path, then use folder-structure based on it: folder ```path```, inside
  folder ```to```, then inside folder ```controller```.
* Place under controller next folders:
    * ```_functional``` - functional endpoint tests, one per endpoint.
    * ```_security``` - security tests, can have test names based on security structure.
    * ```_validation``` - validation endpoint test, one per endpoint.
* Folder ```_interoperation``` should contain tests, which validates interoperation between different controller endpoints. 
