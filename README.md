# Ping-API Test Script Examples


Ping-API provide a service let you could write scripts in JavaScript or CoffeeScript to test your API.  
Your test scripts will automatically run on global servers to test API, such as U.S., Japan, Germany and more...


+ [First test](#first-test)
+ [Initial Script](#initial-script)
+ [Test Script](#test-script)
+ [Storage](#storage)
+ [Variable API path](#variable-api-path)
+ [Set default User-Agent](#set-default-user-agent)


---


## First test
After you create the project, then you can click the `New test` button to create a `test`.  
You can set the api the uri and the method of the test.  
Then you can write the test script like this.
#### Test Script: GET /
```js
this.testCase = function (test, response) {
    test.expect(1);  // how many expectations in this test case.
    test.equal(response.statusCode, 200);  // the response status code should be 200
    test.done();
};
```


## Initial Script
There is one initial script of the each project.  
The initial script will be run before the each test. You can set common functions at here.


## Test Script
There are two section of the test case `setup` and `test`. The setup code should be a function and the prefix is `setup`. The test code should be a function and the prefix is `test`.
#### Test Script: POST /login
```js
this.setupCaseA = function () {
    /*
    The setup function of CaseA. This is optional.
    @return {object}
        params: {object}
            The url query string.
        headers: {object}
        body: {object|string}
            If the body is object Ping-API will convert to JSON at default.
            Set 'Content-Type': 'x-www-form-urlencoded' at headers Ping-API will convert to unlencoded form.
    */
    return {
        params: {
            index: 0
        },
        headers: {
            'User-Agent': 'Chrome'
        },
        body: {
            account: 'test-account',
            password: 'password'
        }
    };
};
this.testCaseA = function (test, response) {
    /*
    The test function of CaseA.
    @params test {Test}
        expect: {function} arguments: num{number}
            How many assertions should be passed.
        equal: {function} arguments: actual{any}, expected{any}, message{string}
            Test with the equal comparison operator ( === ).
        done: {funnction}
            Call this method when the test case was done.
    @params response {Response}
        statusCode: {number}
        headers: {object}  // all keys are lowercase
        body: {string}
        request: {object}
    */
    test.expect(2);
    test.equal(response.statusCode, 200, 'response status should be 200.');
    var content = JSON.parse(response.body);
    test.ok(!content.error, 'server should not return error messages.');
    test.done();
};
```


## Storage
You can use `storage` to pass variable to the next test.
#### Test Script: POST /login
```js
this.setupCase = function () {
    return {
        body: {
            account: 'test-account',
            password: 'password'
        }
    };
};
this.testCase = function (test, response) {
    test.equal(response.statusCode, 200);
    var content = JSON.parse(response.body);
    test.ok(content.token);
    storage.token = content.token;
    test.done();
};
```
#### Test Script: GET /me
```js
this.setupCase = function () {
    return {
        params: {
            token: storage.token
        }
    };
};
this.testCase = function (test, response) {
    test.equal(JSON.parse(response.body), {
        account: 'test-account',
        id: 1
    });
    test.done();
};
```


## Variable API path
#### Test Script: GET /users/{userId}
```js
this.setupCase = function () {
    return {
        params: {
            userId: '550e8400-e29b-41d4-a716-446655440000'
        }
    };
};
this.testCase = function (test) {
    test.done();
};
```


## Set default User-Agent
All of test requests will append this user-agent at header.
#### Initial Script
```js
storage.headers = {
    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5) Chrome/46.0.2490.80'
};
```
