# Files and folders structure

### Naming

Use ```TestName.test.ts``` as base test name.

### Base folder structure

```
🗀 src
🗁 test
   🗀 _featur
   🗀 main_api
   🗀 second_api
package.json
```

* ```🗀 src``` contains classes used for tests but not tests themselves. Put here API client, utils etc.
* ```🗀 test/_features``` feature tests; use exactly this folder name;
* ```🗀 test/main_api``` tests for "main API"
* ```🗀 test/second_api``` tests for "second API"

Underscore character (```_```) is used to:

* Identify standard folders.
* Place them on top of the alphabetical sorting.

### Feature tests

Folder ```🗀 test/_feature``` should contain feature testing.

It is for complex flow testing, which can involve using many endpoints from many services.

### API tests

Each API should have its folder for tests. For example, if your application contains two APIs named "main" and "
second," use folders ```🗀 test/main_api``` and ```🗀 test/second_api```.

**Internal structure**

```
🗁 src
   api.client.ts
   utils.ts
🗁 test
   🗁 main_api
      🗁 _feature
         Feature1.test.ts
         Feature2.test.ts
      🗁 path
         🗁 to
            🗁 controller
               🗁 _functional
                  GET.endpoint1Name.test.ts
                  POST.endpoint2Name.test.ts
                  PUT.endpoint3Name.test.ts
                  PUT.endpoint3Name.CaseName.test.ts
               🗁 _security
                  Roles.test.ts
                  Other.test.ts
               🗁 _validation
                  GET.endpoint1Name.test.ts
                  POST.endpoint2Name.test.ts
                  PUT.endpoint3Name.test.ts
               🗁 _interoperation
                  FeatureInteroperatesWithOtherFeature.test.ts
               🗁 subpath
                  🗀 _functional
                  🗀 _security
                  🗀 _validation
               ControllerCommons.ts
      🗀 controller2path
      🗀 controller3path
  🗀 second_api
```

* When ```/path/to/controller``` is the controller path, use folder structure based on it: folder ```🗀 path```, inside
  a folder ```🗀 to```, then inside folder ```🗀 controller```.
* Place under controller next folders:
    * ```🗀 _functional``` - functional endpoint tests, one per endpoint. Optionally special cases can be written in additional files - per endpoint or controller based on a specific case.
    * ```🗀 _security``` - security tests can have test names based on security structure.
    * ```🗀 _validation``` - validation endpoint test, one per endpoint.
    * ```🗀 _interoperation``` should contain tests which validate interoperation between different
      controllers/features.

### Test file naming

* For endpoint test: ```METHOD.CONTROLLER_PATH.(optional:CASE_NAME).test.ts```
    * ```GET.action.test.ts```
    * ```POST.profile.test.ts```
    * ```POST.block.test.ts```
    * ```POST.block.SpecialCase.test.ts```
* For interoperation test: ```INITIATOR.RESPONDER.test.ts```
    * ```UserBlock.UserProfile.test.ts```
    * ```UserBlock.UserPosts.test.ts```
* For feature and case test: ```FEATURE_NAME/CASE_NAME.test.ts```
    * ```Authorization.test.ts```
    * ```RolesAndPermissions.test.ts```

### Test set naming

* For endpoint test: ```METHOD + [PATH] + TYPE```
    * ```GET [/path/to/controller/action] functional```
    * ```POST [/users/profile] validation```
    * ```POST [/users/admin/block] security```
    * ```POST [/users/admin/block] feature: some special case```
* For interoperation test: ```INITIATOR + RESPONDER```
    * ```GET [/path/to/controller/action] functional```
    * ```POST [/users/profile] validation```
    * ```POST [/users/admin/block] security```
    * ```POST [/users/admin/block] feature: some special case```
* For feature test: ```FEATURE_NAME```
  * ```Authorization```
  * ```User purchase flow```
* For controller case test: ```[CONTROLLER_PATH] + CASE_NAME```
  * ```[/path/to/controller] Roles and permissions```
