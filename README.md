# Ping-API Test Script Examples


Ping-API provide a service let you could write scripts in JavaScript or CoffeeScript to test your API.  
Your test scripts will automatically run on global servers to test API, such as U.S., Japan, Germany and more...



## First test
After you create the project, then you can click the `New test` button to create a `test`.  
#### Test Script
```js
this.testCode = function (test, response) {
    test.expect(1);  // how many expectations in this test case.
    test.equal(response.statusCode, 200);  // the response status code should be 200
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
